I can't believe it took me so long to think of this. But a digital Zettelkasten has one major drawback compared to the physical one. Lack of proximity. Notes are not any closer or further apart to any other Note. Using tags to track location, i.e. next note and previous is only useful if you can surface those tags visually. Like looking into a shoebox and flipping through index cards. At a glance you know that the cards in one section "belong together". Currently even with bases I don't think there is an easy way to surface that at a glance inference so instead I'm using this note.
Each heading will be a topic and or subtopic. Something specific enough that it can be the thesis of an essay or the launching point into an area of interest.

Limitations (for me):
- A note may appear in this file at most once.
- These links are for literature notes to surface my research essentially.
- Headings must be broad knowledge categories
- Sub heading are narrower ones
- Finally a H3 heading that is the question/thesis we are answering -> will hopefully point to a new permanent note that uses the notes listed below it in this file as sources.

Writing and Thinking:
- [[Writes and Write-Nots]]
- [[Coming home]]
- [[As a developer, my most important tools are a pen and a notebook]]
- [[Thoughts on thinking]]
- [[The IndieWeb Doesn't Need to Take Off]]

Learning:
- [[You are NOT Dumb, You Just Lack the Prerequisites]]
- [[You Must Read At Least One Book To Ride]]

Theory Crafting:
- [[Software Design is Knowledge Building]]

Indieweb:
- [[Stop Using Fandom]]
- [[Thoughts on the Resiliency of Web Projects]]
- [[A website to destroy all websites.]]

blogging (more practical):
- [[Advice for a friend who wants to start a blog]]
- [[Writing a tech blog people want to read]]

local-first app development:
- [[Linear sent me down a local-first rabbit hole]]
- [[Local-First & Ejectable]]
- [Local-first software](https://www.inkandswitch.com/essay/local-first/) <- the manifesto essentially

coding practices:
- [[The Perfect Commit]]

Restructuring mental models out of OOP pitfalls:
- ZII: Zero as Initialization
- ECS: Entity Component System (Composition over Inheritance, OOP encapsulation around a system not an object)
  [breakdown](https://softwareengineering.stackexchange.com/questions/186696/is-it-reasonable-to-build-applications-not-games-using-a-component-entity-syst)
  [the case for it](https://www.youtube.com/watch?v=wo84LFzx5nI)
  Go package is a process -> verb based booking system which does everything plus interface. pacage can be an adapter such as http at the boundary or infrastructure that says HOW something is done such as postgres package to handle the other side of the app boundary [[ECS full stack]].
- Discriminated Unions, tagged union or sum type
- Hexagonal Architecture?
- More generic term for ECS is data oriented design ([primer](https://gamesfromwithin.com/data-oriented-design), [book](https://www.dataorienteddesign.com/dodbook/))
- Process oriented (procedural) programming
- In classic contexts the world is the datastore (in small projects we can keep it in memory)

Data Design (WIP)
- Need to check if map reduce is still relevant (check Intro to 2nd edition of Designing Data Intensive Applications)
- Collection of data oriented design [links](https://github.com/dbartolini/data-oriented-design)
- https://github.com/kousen/dataorientedprogramming/blob/master/src/main/java/com/kousenit/anthropic/ClaudeRecords.java (an example for http client of a rest api with java)
- https://www.youtube.com/watch?v=bUOOaXf9qIM talk on making a high performant native app from scratch, implicitly follows DoD.
- Struct of Arrays
- Swapback Array
- Cache misses?
- Out of band?
- Sparse set pattern?
- Registry pattern?
- Memory management with arenas
- Encoding strategy?

CLI design and implementation
- https://bettercli.org/ -> website decdicated to the desgin of CLIs
- https://theleo.zone/posts/pager/ -> interesting and helpful breakdown on viewport in TUI w/ bubbletea

Luck as a surface
- https://tantaman.com/2026-01-27-manufacturing-luck.html

Parsing as validation
 - [[Parse, don’t validate]]
 - [[Parsing data is nicer than only validating it]]
 - [[Excessive nil pointer checks in Go]] same idea of making undesired state impossible at the boundary rather than continuously handling and checking throughout the program

Tech Governance
- IETF -> internet protocol. HTTP, QUIC, TLS, ...
- W3C -> standards body. HTML, CSS, XML, ...
- WHATWG -> also HTML? they made HTML5. Living standards not RFC based. Started by browsers.
- Unicode Consortium
- ICANN and IANA -> administrator of domains and DNS

Fast software
- [[Fast Software, the Best Software]]
- [[fast]]

### Structs of Arrays
Column stores look like struct of arrays???
Classic Row based table
```
[id_1, name_1, age_1]
[id_2, name_2, age_2]
[id_3, name_3, age_3]
```
Column based table
```
[id_1, id_2, id_3]
[name_1, name_2, name_3]
[age_1, age_2, age_3]
```
Classic Array of Structs
```c
struct Vector3 {
    float x;
    float y;
    float z;
};
struct Vector3 points[N];

float get_point_x(size_t i) {
    return points[i].x;
}
```
Struct of Array
```c
struct Vector3List {
    float x[N];
    float y[N];
    float z[N];
};
struct Vector3List points;

float get_point_x(size_t i) {
    return points.x[i];
}
```

[[2000 words about arrays and tables]] the math of it i guess. Argues that arrays and table and tables are just functions actually

ChatGPT help
```
The useful programmer mental model is:

Struct of arrays = “turn fields into columns.”

The article frames arrays as functions from indexes to values, like:

index -> value

A normal array of structs is:

i -> { x: number, y: number, alive: boolean }

A struct of arrays flips that into:

{
  x:     i -> number,
  y:     i -> number,
  alive: i -> boolean
}

Hillel’s key point: this works because a table/row is “array index + named fields,” and an array-of-structs is isomorphic to a struct-of-arrays. You can access by row or by column.

For a programmer, the insight is: choose the layout that matches the question you ask most often.

If your code usually says:

for entity in entities:
  entity.position.x += entity.velocity.x

array-of-structs feels natural.

But if your code usually says:

for i in entities:
  x[i] += vx[i]

struct-of-arrays is natural, and often faster because each loop touches dense same-typed memory.

A good mental model:

// Array of structs: row-first
players[i].health
players[i].position
players[i].name

// Struct of arrays: column-first
players.health[i]
players.position[i]
players.name[i]

The article’s deeper idea is that homogeneous fields can become dimensions, but heterogeneous fields cannot fully become one clean multidimensional array. RGB pixels can be treated like:

row -> col -> colorChannel -> int

because R, G, and B are all ints. But airport data like name, flights, and revenue cannot become one clean axis because those fields have different types. Instead, you split it into columns: name column, flights column, revenue column.

So the practical takeaway:

Struct-of-arrays is not just an optimization trick. It is a change in perspective from “I have many objects” to “I have several aligned datasets sharing the same index.”

Use it when:

you process one/few fields across many items;
you want cache-friendly tight loops;
you want SIMD/vectorization;
you are building ECS/game/data-processing systems;
you think in columns, filters, masks, joins, or batches.

Avoid it when:

you frequently need whole individual objects;
records have lots of optional/irregular fields;
clarity matters more than batch performance;
object identity/lifetimes dominate the design.

The programmer-friendly slogan:

AoS is object thinking. SoA is table thinking.
```

