---
tags:
  - type/chapter
  - status/dark
created: 2024-11-25
---
[[Mastering API Architecture]]
# Design, Build, and Specify APIs

> [!abstract] Summary
## Note
---
> [!tip]- Table of Contents
> 1. [[#Introduction to REST]]
> 	1. [[#Introduction to REST and HTTP by Example]]
> 	2. [[#The Richardson Maturity Model]]
> 2. [[#Introduction to Remote Procedure Call (RPC) APIs]]
> 3. [[#A Brief Mention of GraphQL]]
> 4. [[#REST API Standards and Structures]]
> 	1. [[#Collections and Pagination]]
> 	2. [[#Filtering Collections]]
> 	3. [[#Error Handling]]
> 	4. [[#ADR Guideline Choosing an API Standard]]
> 5. [[#Specifying REST APIs Using OpenAPI]]
> 6. [[#Practical Application of OpenAPI Specifications]]
> 	1. [[#Code Generation]]
> 	2. [[#OpenAPI Validation]]
> 	3. [[#Examples and Mocking]]
> 	4. [[#Detecting Changes]]
> 7. [[#API Versioning]]
> 	1. [[#Semantic Versioning]]
> 	2. [[#OpenAPI Specification and Versioning]]
> 8. [[#Implementing RPC with gRPC]]
> 9. [[#Modeling Exchanges and Choosing an API Format]]
> 	1. [[#High-Traffic Services]]
> 	2. [[#Large Exchange Payloads]]
> 	3. [[#HTTP/2 Performance Benefits]]
> 	4. [[#Vintage Formats]]
> 10. [[#Guideline Modeling Exchanges]]
> 11. [[#Multiple Specifications]]
> 	1. [[#Does the Golden Specification Exist?]]
> 	2. [[#Challenges of Combined Specifications]]

### Introduction to REST
- [[REST]] is an API architecture that meets these criteria:
	- Producer models resources a consumer can interact with.
	- Requests are [[Computer State|stateless]], they don't care about any previous requests.
	- Requests can be [[Caching|cached]].
	- An [[interface]] is presented to the consumer.
	- [[Abstraction|Abstracts]] away complexity behind the interface.
#### Introduction to REST and HTTP by Example
- REST uses verbs to describe actions that can be taken on a resource. `GET` describes "getting" a resource.
- REST defines the content in the body and places [[metadata]] into the header of the request.
- The response includes an HTTP [[status code]] and a message from the server in the header.
- The response body holds the requested content, usually as [[JSON]].
#### The Richardson Maturity Model
- The [[Richardson Maturity Model]] defines to what level an API follows the REST criteria:
	- Level 0 (HTTP/RPC): The API uses HTTP but applies no verb to tne endpoint, essentially making it an RPC implementation. (`/attendees).
	- Level 1 (Resources): The API uses and models resources in the [[URI]]. (`GET attendees/1`).
	- Level 2 (Verbs): The API models multiple resource URIs depending on the effect the verb has on the server, `GET` vs `PUT`. (`GET attendees/1` `PUT attendees/1`).
	- Level 3 (Hypermedia Controls): These are navigable APIs using [[HATEOAS]]. This level is rarely implemented in modern REST services.
- Level 2 is a good standard to aim for when designing APIs.
### Introduction to Remote Procedure Call (RPC) APIs
- [[Remote Procedure Call]] (RPC) is an API architecture that calls a method in one process, and executes it in another.
- RPC involves exposing a method to be called rather than a resource to be used,
- [[gRPC]] is the de facto industry standard.
  Figure 1
- gRPC requires schema to fully define the interaction between producer and consumer.
- RPC can have state as part of its implementation, which can lead to tighter [[coupling]]
### A Brief Mention of GraphQL
- [[GraphQL]] is a technology that "lays" over the rest of the stack: APIs, Databases and Services.
- It provides its own query language to be used to query multiple sources that it "lays" over.
- This allows the client to query multiple API and/or their combinations easily.
- It is designed for software that needs to access large amounts of data that is "spread" out across many subsystems. It is best used as a complementary technology.
### REST API Standards and Structures
- When designing, [[API Guidelines]] are essential for creating uniformity across an implementation and standardising certain design decision.
- Naming correctly and in a standardised manner is crucial to prevent breaking changes in API development.
#### Collections and Pagination
- Array responses should be returned inside an object as this allows for the modelling of collections and pagination.
#### Filtering Collections
- Filtering standards are best decided early on to maintain easier compatibility.
#### Error Handling
- Error standards are essential for upfront design and consistency.
- Status codes MUST be accurate as they imply a certain type of fault to the developer.
#### ADR Guideline: Choosing an API Standard
- When making a decision on API standards pick a guideline that matches the style of the API being developed. Write it up in an ADR (Architecture Decision Record).
### Specifying REST APIs Using OpenAPI
### Practical Application of OpenAPI Specifications
#### Code Generation
#### OpenAPI Validation
#### Examples and Mocking
#### Detecting Changes
### API Versioning
#### Semantic Versioning
#### OpenAPI Specification and Versioning
### Implementing RPC with gRPC
### Modeling Exchanges and Choosing an API Format
#### High-Traffic Services
#### Large Exchange Payloads
#### HTTP/2 Performance Benefits
#### Vintage Formats
### Guideline: Modeling Exchanges
### Multiple Specifications
#### Does the Golden Specification Exist?
#### Challenges of Combined Specifications

## Citation
---
```
J. Gough, D. Bryant, M. Auburn, "Design, Build, and Specify APIs" in *Mastering API Architecture*, 1st. M. Duffield, V. Wilson, C. Laylock, K. Cofer, Eds. Sebastopol, CA, USA: O'Reilly, 2022, pp. 41-63.
```

---

##### Completion Checklist
###### II. From Dark to Dawn
- [x] Wait at least 5mins before beginning this section.
- [ ] In bullet points, under each section and sub section heading summarise the major points of that section. In these bullet point make links to anything that could be referenced in a permanent note. This will take awhile.
- [ ] While summating the sections copy paste interesting sections into the note under `Highlights`. Include the page number right above the block. These highlights should only be the author's own reflections that you think are interesting, nothing definitive like a statement of fact. There may be nothing.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ]  **Bold** the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Write the chapter `Summary`.
- [ ] Recreate any figures in Excalidraw if possible, link them in the note.
- [ ] Remove or complete the `Nota Bene` as necessary.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.