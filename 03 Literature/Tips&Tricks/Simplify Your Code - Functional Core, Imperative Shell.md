---
tags:
  - tipsntricks
source: https://testing.googleblog.com/2025/10/simplify-your-code-functional-core.html
links:
  - "[[testing]]"
  - "[[software architecture]]"
---
# Simplify Your Code: Functional Core, Imperative Shell

> [!abstract] Summary
> Split your programs into a functional core and an imperative shell. The shell should only call on the functional core and generate side effects. The core should only operate on data passed in. This pattern allows for better, simpler code.
## Why

1. **Maintainable.** Splitting code into a functional core and an imperative shell makes it easier to maintain as it decouples those different behaviours.
2. **Testable.** Isolating side effects from business logic makes the code more testable as it can check against business logic and side effects separately.
3. **Adaptable.** This divide makes the code more adaptable. We can update either the business logic or the shell without affecting the other.
## How

1. **Functional core.** Functions that only operate on the data being passed in, this is your business logic.
2. **Imperative shell.** Functions that call on the functional core to handle its logic but only directly output side effect. Such as database calls or sending emails.
### Bad pattern

```
function sendUserEmail() {
	for user in users:
		if (check subscription end) <behaviour> // logic
		if (check free trial) <behaviout // logic
		email.send(data + "some content") // side effect
}
```
### Good pattern

```
function getExpired(data) {
	if (check subscription end and check free trial) <behaviour> // logic
}

function generateExpiredEmails(data) {
	add some content to data // logic
}

function bulkSend() {
	get data
	apply logic functions
	send email // side effect
}
```