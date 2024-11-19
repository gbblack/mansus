---
tags:
  - literature/web_article
  - software_architecture
  - frontend
  - status/day
source: https://jasonformat.com/islands-architecture/
created: 2024-11-18
---
# Islands Architecture

> [!abstract] Summary
> A primer on a frontend web architecture called "Component Islands". The idea of this architecture is to serve your app as HTML pages with "placeholder" islands that contain the dynamic parts of your app.
### Highlights
---
> The general idea of an “Islands” architecture is deceptively simple: **render ==HTML pages on the server==, and inject ==placeholders== or slots around highly ==dynamic regions.==**

> You can think of this like **a ==static HTML== document that contains multiple ==separate embedded applications==**

> A closer analog to the "islands" approach would be progressive enhancement, to which we're essentially adding SSR hydration and a consistent metaphor for ==**adding interactivity to a region== of the page.**

> rendering pages using an islands architecture results in the heavier **dynamic portions of the page being initialized not just progressively, but ==_separately==_.** This means individual regions of the page become interactive without anything else on the page needing to be loaded first.

> **Each part of the page is an ==isolated unit**==, and a performance issue in one unit doesn't affect the others.

> The HTML returned in response to navigation **contains a ==meaningful== and immediately renderable representation of the content the user requested.**

> the document should at least contain **the most ==essential content==.** For example: a news page’s HTML would contain the article body

> **All of the ==other content is secondary== to this information**, and its inclusion in the HTML becomes a product decision. How vital is this bit of information to a user visiting the page? How important is that widget to the business model?
---
##### Citation
```
"Islands Architecture." JASON Format. https://jasonformat.com/islands-architecture/
```