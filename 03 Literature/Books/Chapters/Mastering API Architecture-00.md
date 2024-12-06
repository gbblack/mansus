---
tags:
  - type/book_chapter
  - status/day
created: 2024-11-25
---
[[Mastering API Architecture]]
# **Introduction**

> [!abstract] Summary
> Modern API Architecture must be able to handle an ever increasing set of requirements without needing constant refactoring. To achieve this an evolutionary architecture approach should be implemented and well documented.
### **Note**
---
##### **The Architecture Journey**
- Architecture changes over time due to varying influences: technology, business requirements and constraints.
- Adding incremental value to the software architecture overtime leads to [[Evolutionary Architecture|evolutionary architecture]]. 
- With the shift to [[SOA]], APIs have become inherently evolutionary.
- [[API-First]] design is the resultant approach to implement this evolutionary architecture.
##### **A Brief Introduction to APIs**
- [[API|APIs]] fall into two categories: *in process* and *out of process*.
- [[in process]] is an internal API invocation where the process remains the same after the call (e.g. a function invoking another).
- [[out of process]] is an external API invocation where the process calls upon another different process (e.g. An HTTP request to another service).
##### **Running Example: Conference System Case Study**
![[MasteringAPIArchitecture-0001.png]]
![[MasteringAPIArchitecture-0002.png]]

- The [[API Controller]] handles all the requests and routes them to the appropriate module.
- This follows the [[front controller pattern]] which uses many in process invocations.
- The other components use out of process invocations to handle queries to one another.
![[MasteringAPIArchitecture-0003.png]]
**Types of APIs in the Conference Case Study**
- Web App -> API Controller is out of process
- API Controller -> Attendee Component is in process.
- Anything within Conference Application is in process.
**Reasons for Changing the Conference System**
- The system must be updated to handle new requirements without starting over each time.
**From Tiered Architecture to Modeling APIs**
- The original architecture is an example of [[Three-Tier Architecture|three-tier architecture]].
- It needs to be updated into an evolutionary architecture.
- The way to do this is by modeling [[API Interactions|API interactions]], especially [[traffic patterns]].
**Case Study: An Evolutionary Step**
![[MasteringAPIArchitecture-0004.png]]
- [[North-south traffic]]: is an [[ingress flow]], where interactions involves a public facing component.
- [[East-west traffic]]: is a [[service-to-service]] way of communicating, where interactions involve only components that are part of the same system.
##### **API Infrastructure and Traffic Patterns**
- There are two main infrastructure components for [[traffic management]]: [[API Gateways]] for north-south traffic and [[service-mesh]] (Kubernetes) for east-west traffic.
##### **Using C4 Diagrams**
- [[C4 model]] is a good documentation standard for architecture.
**C4 Context Diagram**
- This diagram (1) is to provide context for all stakeholders, both technical and nontechnical.
**C4 Container Diagram**
- This diagram (2) is to describe the major participants inside a component.
**C4 Component Diagram**
- This diagram (3) is to define the roles and responsibilities inside each container and their interactions. This can be a good map to a codebase.
##### **Using Architecture Decision Records**
- [[Architecture Decision Records|ADRs]] are a way of documenting decisions to maintain clarity in software over time.
- There are four sections of ADR: status, context, decision, and consequences.
- Each proposed ADR must eventually be either accepted or rejected.
- They must be made public for easy review and feedback while the ADR is being considered.
![[MasteringAPIArchitecture-0005.png]]
**Mastering API: ADR Guidelines**
- [[ADR Guidelines]] provide context for architecture decision to help the design process.
### **Highlights**
---
25
> **Architecture is a ==journey without a destination==**, and you cannot predict how technologies and architectural approaches will change.

26
> **The ==culminating effect of delivering incremental value combined== with new emerging technologies leads to the concept of evolutionary architecture.** Evolutionary architecture is an approach to incrementally changing an architecture, focusing on the ability to change with speed and reducing the risk of negative impacts.
##### **Citation**
---
```
J. Gough, D. Bryant, M. Auburn, "Introduction" in *Mastering API Architecture*, 1st. M. Duffield, V. Wilson, C. Laylock, K. Cofer, Eds. Sebastopol, CA, USA: O'Reilly, 2022, pp. 25-37.
```
