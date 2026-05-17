```
Monorepo
  ├── contracts/        API boundary definitions
  ├── server/           Go ECS-style backend
  └── client/           MVVM frontend with ECS-ish client state
```

Backend ECS = authoritative business state and rules
Frontend MVVM = UI projection over backend state
Client ECS-ish state = local cache + pending user intents
Contracts = shared boundary, not shared internals

```
repo/
  contracts/
    openapi.yaml

  server/
    go.mod
    cmd/api/main.go

    internal/
      world/
        entity.go
        world.go

      booking/
        components.go
        systems.go
        ports.go

      membership/
        components.go
        systems.go

      payment/
        components.go
        systems.go

      http/
        booking_handler.go

      postgres/
        booking_store.go

  client/
    src/
      booking/
        components.ts
        bookingViewModel.ts
        bookingSystems.ts
        BookingScreen.tsx

      api/
        bookingApi.ts

      store/
        world.ts
```

## Encapsulation boundary of a “System”

In this context, a system is not “a class” or “a service layer”. It is a package-level business process boundary.

Example backend systems:

```
booking/
  owns booking state transitions:
    BookingRequested → BookingConfirmed
    BookingRequested → BookingRejected
    BookingCancelled → WaitlistPromoted
membership/
  owns membership eligibility
payment/
  owns payment authorization/capture
notification/
  owns dispatching messages
```

A system encapsulates:

**Inputs:**
  components/events it reads

**Rules:**
  business logic

**Outputs:**
  new components/events

**Ports/interfaces:**
  capabilities it needs from outside

It should not expose all internals. Other packages cross the boundary through explicit interfaces or commands/events.
## Backend ECS mental model

Entity    = identity
Component = plain data attached to an entity
System    = behavior that processes entities with certain components
World     = component storage/query mechanism
Small Go example
### Entity
```go
package world

type EntityID string

func NewEntityID() EntityID {
	return EntityID(uuid.NewString())
}
```

An entity is just identity. It should not have behavior.

### Components
```go
package booking

import (
	"time"
	"gym/server/internal/world"
)

type BookingRequested struct {
	UserID        world.EntityID
	ClassID       world.EntityID
	IdempotencyKey string
	RequestedAt   time.Time
}

type BookingConfirmed struct {
	BookingID   world.EntityID
	UserID      world.EntityID
	ClassID     world.EntityID
	ConfirmedAt time.Time
}

type BookingRejected struct {
	UserID  world.EntityID
	ClassID world.EntityID
	Reason string
}
```

These are structs. Mostly no methods.

They answer: “What state/fact exists?”
not: “What behavior can this object perform?”
### Interfaces / ports

Interfaces live near the system that consumes them.

```go
package booking

import "gym/server/internal/world"

type MembershipChecker interface {
	IsActive(userID world.EntityID) (bool, error)
}

type CapacityChecker interface {
	HasSpace(classID world.EntityID) (bool, error)
	ReserveSpace(classID world.EntityID) error
}

type BookingStore interface {
	SaveConfirmed(booking BookingConfirmed) error
	SaveRejected(rejection BookingRejected) error
}
```

The booking package defines what it needs. postgres, membership, etc. provide implementations.

### System behavior
```go
package booking

import (
	"time"

	"gym/server/internal/world"
)

type BookingSystem struct {
	World       *world.World
	Memberships MembershipChecker
	Capacity    CapacityChecker
	Store       BookingStore
}

func (s *BookingSystem) Run() error {
	requests := s.World.Query[BookingRequested]()

	for _, entity := range requests {
		req := s.World.Get[BookingRequested](entity)

		active, err := s.Memberships.IsActive(req.UserID)
		if err != nil {
			return err
		}

		if !active {
			rejected := BookingRejected{
				UserID:  req.UserID,
				ClassID: req.ClassID,
				Reason: "inactive membership",
			}

			s.World.Add(entity, rejected)
			_ = s.Store.SaveRejected(rejected)
			continue
		}

		hasSpace, err := s.Capacity.HasSpace(req.ClassID)
		if err != nil {
			return err
		}

		if !hasSpace {
			rejected := BookingRejected{
				UserID:  req.UserID,
				ClassID: req.ClassID,
				Reason: "class is full",
			}

			s.World.Add(entity, rejected)
			_ = s.Store.SaveRejected(rejected)
			continue
		}

		if err := s.Capacity.ReserveSpace(req.ClassID); err != nil {
			return err
		}

		confirmed := BookingConfirmed{
			BookingID:   world.NewEntityID(),
			UserID:      req.UserID,
			ClassID:     req.ClassID,
			ConfirmedAt: time.Now(),
		}

		s.World.Add(entity, confirmed)
		_ = s.Store.SaveConfirmed(confirmed)
	}

	return nil
}
```

This is where business behavior lives.

## Where struct behavior belongs
### Entity
No behavior.

```go
type EntityID string
```

Maybe constructors/helpers only.

### Component structs

Usually no behavior.

Allowed:

```go
func (c ClassCapacity) SpotsLeft() int {
	return c.Max - c.Booked
}
```
Avoid:

```go
func (b BookingRequested) Confirm() BookingConfirmed
```

because that is business transition logic and belongs in a system.

### Systems

Business behavior lives here.

```go
func (s *BookingSystem) Run() error
```

Systems coordinate components, rules, and ports.

### HTTP DTOs

Marshalling behavior lives at the edge.

```go
type RequestBookingDTO struct {
	ClassID string `json:"classId"`
}
```

HTTP handler translates DTO into ECS intent:

```go
world.Add(entity, booking.BookingRequested{...})
```

The handler should not decide business rules.

### Infrastructure structs

Persistence/payment/email behavior lives here.

```go
type PostgresBookingStore struct {
	DB *sql.DB
}

func (s *PostgresBookingStore) SaveConfirmed(b booking.BookingConfirmed) error {
	// SQL here
	return nil
}
```

## Frontend MVVM + ECS-ish client state

### Frontend structure

```
booking/
  components.ts        client-side state
  bookingSystems.ts    client-side behavior
  bookingViewModel.ts  screen projection/actions
  BookingScreen.tsx    view
```

Frontend responsibilities:

```
View:
  renders UI

ViewModel:
  derives display state
  exposes user actions

Client systems:
  send commands to backend
  apply server responses
  manage pending/error/optimistic UI state

API client:
  HTTP boundary

Server:
  authoritative truth
```

Flow:

```
User clicks Book
→ View calls ViewModel.book()
→ ViewModel adds BookingRequested intent
→ Client system sends RequestBookingCommand
→ Server ECS processes it
→ Server returns BookingConfirmed / BookingRejected
→ Client applies result to local state
→ ViewModel updates
→ View re-renders
```

## Frontend example mental mapping
```ts
// client/src/booking/components.ts

export type BookingRequested = {
  classId: string
  idempotencyKey: string
}

export type BookingPending = {
  classId: string
}

export type BookingConfirmed = {
  bookingId: string
  classId: string
}

export type BookingRejected = {
  classId: string
  reason: string
}
```

ViewModel:

```ts
export function useBookingViewModel(classId: string) {
  const pending = world.exists("BookingPending", { classId })
  const confirmed = world.exists("BookingConfirmed", { classId })

  return {
    canBook: !pending && !confirmed,
    buttonText: pending ? "Booking..." : confirmed ? "Booked" : "Book class",

    book() {
      world.add(newEntity(), "BookingRequested", {
        classId,
        idempotencyKey: crypto.randomUUID(),
      })
    },
  }
}
```

The ViewModel does not confirm the booking. It only expresses intent.

## Contract boundary

Use `contracts/openapi.yaml` for boundary DTOs:

```
RequestBookingCommand
BookingView
BookingConfirmedResponse
BookingRejectedResponse
```

But do not expose:

```
World
Systems
Private backend components
Database models
UI-only frontend components
```

The contract is only for crossing the client/server boundary.

## Final mental model

```
Backend packages = business capability boundaries

booking package:
  owns booking transitions

membership package:
  owns eligibility

payment package:
  owns payment state

http package:
  translates HTTP into commands/intents

postgres package:
  persists state

client ViewModel:
  presents backend state to UI

client systems:
  synchronize UI state with backend commands/responses
```

The cleanest rule:

```
Components describe state.
Systems own behavior.
Interfaces describe capabilities crossing package boundaries.
DTOs describe data crossing network boundaries.
Views render projections.
ViewModels translate user actions into intents.
```