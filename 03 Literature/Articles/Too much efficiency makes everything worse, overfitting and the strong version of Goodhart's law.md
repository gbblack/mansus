---
tags:
  - type/article
  - status/dawn
  - anti_efficiency
  - technical_philosophy
author:
  - Jascha Sohl-Dickstein
publication:
  - Jascha's Blog
source: https://sohl-dickstein.github.io/2022/11/06/strong-Goodhart.html
created: 2025-01-07
---
# **Too much efficiency makes everything worse: overfitting and the strong version of Goodhart's law**

> [!abstract] Summary
> An article comparing the problem of overfitting (and its solutions) in machine learning apply to the real world focus on efficiency.
### **Highlights**
---
> ==**Increased efficiency== can sometimes, counterintuitively, lead to ==worse outcomes.==** This is true almost everywhere. We will name this phenomenon the strong version of Goodhart's law.

> This same **counterintuitive relationship between efficiency and outcome ==occurs in machine learning**==, where it is called overfitting.

> We can't directly fit the model to the goal, so we instead **train the model using ==some proxy which is _similar_ to the goal==.**

> Goodhart's law states that, ==**_when a measure becomes a target, it ceases to be a good measure_.**==

> it describes how the **proxy objective we optimize ceases to be a good measure** of the objective we care about.

> The ==**goal== often starts ==getting _worse_==**, even as our proxy objective continues to improve.

> What about the most common case though — where the thing we are making more efficient is related, but not identical, to beneficial outcomes? What happens when we get better at something which is merely correlated with outcomes we care about?

> **Goal:** Rapid progress in science  
> **Proxy:** Pay researchers a cash bonus for every publication
> **Strong version of Goodhart's law leads to:** Publication of incorrect or incremental results, collusion between reviewers and authors, research paper mills

> **Goal:** Leaders that act in the best interests of the population  
> **Proxy:** Leaders that have the most support in the population  
> **Strong version of Goodhart's law leads to:** Leaders whose expertise and passions center narrowly around manipulating public opinion at the expense of social outcomes

> Better ==**align proxy== goals** with desired outcomes.

> Add ==**regularization penalties**== to the system.
> ...
> anything that **penalizes complexity, or ==adds friction== or extra cost to a system,** can be viewed as regularization.

> ==**Inject noise**== into the system.
> ...
> this involves adding random jitter to the inputs, parameters, and internal state of a model. The ==**unpredictability== resulting from this noise ==makes overfitting far more difficult.**==

> ==**Early stopping.**==
> ...
> besides training loss and test performance, which we call validation loss. When the **==validation loss starts to get worse, we stop training==, even if the training loss is still improving.**

> _causes_ of extreme overfitting is that the ==**expressivity of the model== being trained _too closely ==matches_ the complexity of the proxy task.==**

> When the **model's expressivity roughly matches the task complexity** (e.g., the number of parameters is no more than a few orders of magnitude higher or lower than the number of training examples), **then ==it can only do well on the proxy task by doing _extreme things everywhere else_.**==

> **==Restrict== capabilities / ==capacity.==**
> ...
> making the **model so ==small== that it's ==incapable of overfitting.==** In the broader world, we could similarly limit the capacity of organizations or agents.

> ==**Increase== capabilities / ==capacity.==**
> ...
> this would correspond to **developing ==capabilities== that are ==so great== that there is ==no longer any tradeoff==** required between performance on the goal and the proxy.
##### **Citation**
---
```
J. Sohl-Dickstein, "Too much efficiency makes everything worse: overfitting and the strong version of Goodhart's law", Jascha's Blog.
Available: https://sohl-dickstein.github.io/2022/11/06/strong-Goodhart.html
```