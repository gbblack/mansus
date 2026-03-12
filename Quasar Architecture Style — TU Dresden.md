---
tags:
  - dissertation
author:
source: https://st.inf.tu-dresden.de/files/teaching/ss11/swt/Vorlesungen/46-quasar-2x2.pdf
links:
---
# Quasar Architecture Style — TU Dresden

**Translated by ChatGPT, needs review**

Part IV – Object-Oriented Design (OOD)
46 – Software Architecture with the Quasar Architectural Style
1) Introduction to Object-Oriented Software Architecture

Modularity and the principle of information hiding

Design patterns for modularity

BCD architectural style (3-tier architectures)

Prof. Dr. U. Aßmann – Optional Material
Technical University of Dresden
Institute for Software and Multimedia Technology
Chair of Software Technology
http://st.inf.tu-dresden.de

Version 11-0.1, 10 July 2011

2) Refinement of the Design Model into the Implementation Model

(Enhancement of class diagrams)

Refinement of operations

Refinement of associations

Refinement of inheritance

3) Refinement of Life Cycles

Refinement of different control machines

Cross-cutting refinement using Chicken Fattening

4) Object-Oriented Frameworks
5) Software Architecture with the Quasar Architectural Style

Software Technology
© Prof. Uwe Aßmann
Technical University of Dresden – Faculty of Computer Science

Secondary Literature – Quasar

Johannes Siedersleben (ed.).
Quasar: The sd&m Standard Architecture
http://www.openquasar.de

Johannes Siedersleben.
Modern Software Architecture – Careful Planning, Robust Construction with Quasar
dpunkt Verlag, 2004

Quasar is:

An architectural style developed by SD&M, a leading German company specializing in custom software

Based on:

Software categories ("blood groups")

Component orientation

A-T-I architectural style

Component-oriented development process

Previously we distinguished two aspects of software:

Architecture

Application

Now we additionally distinguish:

Technology

Software “Blood Groups”

(Software categories according to reusability)

0 (Zero)

Independent of application and technology.

Examples:

JDK collections

C++ STL

GNU regular expression libraries

Characteristics:

Interfaces contain only technical types (strings, collections, etc.)

Highly reusable

A (Application)

Application- or domain-specific components derived from the domain model.

Examples:

Client

Customer

Account

Invoice

Characteristics:

Interfaces contain domain types

Components live in:

Application logic layer

Database layer

T (Technology)

Technology-specific APIs that are independent of the application but dependent on the technology.

Examples:

JDBC

CORBA CosNaming

Characteristics:

Interfaces provide technical APIs

Hard to reuse across technologies

Needed everywhere in systems

AT (Application + Technology)

Components that depend on both application and technology.

Characteristics:

Should be avoided

Difficult to maintain

Hard to reuse

Hard to evolve

R (Representation)

Used to convert business objects into external representations and back.

Examples:

Serialization / deserialization

Encryption / decryption

Packing / unpacking

XML tools (e.g., XMI)

Model-driven development tools (OMG MOF)

Transforming objects between languages (e.g., Java → COBOL)

Characteristics:

Special type of A software

Often generated automatically from specifications

Not reusable, but re-generatable

Purpose of the Blood Groups

The goal is to separate application logic and technology as much as possible so they can be reused independently.

Observations:

Technology-oriented components are easier to reuse

Application-specific components are harder to reuse

AT components are problematic

Reusability of Blood Groups

Reusability decreases in this order:

0  →  A  →  T  →  R  →  AT
(high)                    (low)
Siedersleben’s Blood Group Law

Every interface, class, and component must belong to exactly one software category.

These blood groups appear across all layers of the BCED architecture.

At each layer there exist:

Technology components

Application components

Representation components

Blood Group Rules

Calling A-components from T-components is dangerous.

Reason:

It breaks the acyclic USES relationship from Application → Technology

It creates AT software

Blood Group Calculation Rules
A + 0 = A
T + 0 = T
A + T = AT
Siedersleben’s AT Law

Mixing Application (A) and Technology (T) always leads to software that is very difficult to reuse.

What Have We Learned?

Beyond the concepts of architecture vs application and BCED architecture, software components can be categorized into blood groups:

A – Application

T – Technology

0 – Independent

R – Representation

AT – Mixed (problematic)

Guidelines:

Avoid mixing A and T

AT software is poorly reusable

Organize all packages/components in BCED architecture according to blood groups

> [!abstract] Summary
## Highlights
---
9 (page number)
> example highlight
## Citation
---
```
First Name Initial. Last Name, "Name of Paper", level of education. dissertation (or thesis depending), Abbrev. Department., Abbrev. Univ., City of Uni, (US State Only), Country, Year.
```
---
##### Completion Checklist
###### I. To Become Dark
- [ ] Complete the `Citation` section.
- [ ] Write the Full title in the heading.
- [ ] Complete the all metadata except `tags`.
- [ ] Read the paper once in its entirety with focus, no music.
- [ ] Add tag `status/dark`.
###### II. From Dark to Dawn
- [ ] Read the paper again, this time copy pasting interesting sections into the note under `Highlights`.
- [ ] Bold the portions of the `Highlights` you find most interesting.
- [ ] ==Highlight== the best parts of the bolded sections.
- [ ] Update status tag to `status/dawn`.
###### III. From Dawn to Day
- [ ] Write the paper `Summary`.
- [ ] Fill in the context tags for the metadata.
- [ ] Update status tag to `status/day`.
- [ ] Remove this checklist.