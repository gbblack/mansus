---
tags:
  - type/article
  - web_resilience
  - php
  - status/day
author:
  - Aaron Parecki
source: https://aaronparecki.com/2024/08/31/9/too-many-projects
created: 2024-12-11
---
Any Links
# **Thoughts on the Resiliency of Web Projects**

> [!abstract] Summary
> The author's reflection on his old web projects. He finds that projects can become a chore to maintain if they are not dead simple and using basic tech that does not have many dependencies. He sets aside considerations for maintenance before beginning, to help reduce this effect.
### **Highlights**
---
> This is also making me seriously **reconsider the ==value== of spinning up any ==new projects.==**

> it **hasn't needed any ==framework upgrades==** since it's just using PHP templates.

>  I like **==not having== to go through the whole ceremony of ==setting up a dev environment==,** installing dependencies, upgrading things to the latest version, checking for backwards incompatible changes, git commit, deploy, etc.

> Frameworks can **save time in the short term, but have a ==huge cost in the long term.**==

> Databases aren't inherently bad, but using one does **make the project slightly more ==fragile**==

> If a database is required, is it possible to create it in a way that does not result in **ever-growing storage needs?**

> Is this going to store data or be a **service that ==other people== are going to use?** If so, plan on a registration form so that I have a way to **contact people eventually when I need to change it** or shut it down.
##### **Citation**
---
```
"Thoughts on the Resiliency of Web Projects." Aaron Parecki. https://aaronparecki.com/2024/08/31/9/too-many-projects
```