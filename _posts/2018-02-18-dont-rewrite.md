---
layout: post
title:  "Don’t demolish"
date:   2018-02-18 14:00:00 +0100
excerpt: Why a software rewrite is not a good solution, what the underlying issues probably are and what you could do instead.
---

![](/assets/never-demolish.jpg)
*Transformation de 530 logements, bâtiments G, H, I, quartier du Grand Parc - Lacaton & Vassal, Druot, Hutin, Image © [Philippe Ruault](mailto:philippe.ruault@numericable.fr) - For all uses, thank you to contact him*

Much is written about how you should not rewrite a whole application and I have added some further reading at the bottom of this post. Here I want to add some reasons of why you probably landed where you are right now and why a rewrite is just another indicator of your fundamental problems:

<abbr title="Too long didn’t read">TL;DR</abbr>: Software development teams should solve business problems instead of implementing predetermined solutions, need performance indicators and architectural principles.

## An analogy in civil engineering

Housing complexes built in Europe during the 1960s and 1970s are largely considered outdated and urbanistically failed. France ruled in 1990 to rebuild them. Until now, the demolition of 113,200 housing units and the relocation of its inhabitants cost 3 billion. Building 105,000 new homes cost 12 billion - a total of 15 billion for a loss of 8,200 homes.

A couple of architects were offended for economic and social reasons and started to show the government how to renovate instead of demolishing and letting the residents live in their home. There is [a great exhibition](http://ruby-press.com/projects/never-demolish/) about it.

In one example at [Cité du Grand Parc in Bordeaux](http://lacatonvassal.com/index.php?idp=80), three old housing complexes with 4,000 inhabitants were augmented by these architects by adding winter gardens to the front and back extending the living space by one third. Serving as a climate buffer they additionally reduced the house energy consumption by half and added a better working ventilation. The cost came down to half of what a complete rebuild was estimated and renovation was organized in a way to let the residents live inside their homes nearly the whole time.

![](/assets/never-demolish-2.jpg)
*The blue winter gardens were added to the structure for more living space and climate buffer, making a demolition and relocation unnecessary, Image © [Lacaton & Vassal - Druot](https://lacatonvassal.com/index.php?idp=80)*

## Back to software engineering

So you have a "legacy application" which needs a rewrite. Instead of throwing it away and commencing the rewrite ponder about how it could come so far and whether your current organization is really capable of writing something better.

> If your current application is beyond repair in your eyes, the fault most probably lies not within the technology but your organization. Instead of rewriting, think about the prerequisites which are not fulfilled.

## 1. Product development done wrong

> Many software organizations use agile methods nowadays. Agile teams should focus on _business problems_. If you don’t let them, they cannot iterate and you just have a badly implemented waterfall team in disguise.

As an example: You have an online shop and too many abandonments before checkout.

#### Do: Let the team solve business problems

The agile approach is to have an interdisciplinary team e.g. "Customer Journey" and hand them this problem without a solution. Only now is this team in the position to iterate on ever-enhanced solutions: tighten the funnel, make user studies to search for unknown problems, install monitoring, data analysis etc.

![](/assets/Making-sense-of-MVP-5.png)
*Henrik Kniberg (2016) [Source](http://blog.crisp.se/2016/01/25/henrikkniberg/making-sense-of-mvp)*

The team remains active also if there are no current problems which are handed to "Customer Journey". They just enhance the current state of things: Better monitoring, faster performance, more tests, more data analysis, fix small problems before they become big, etc. Top management sometimes focuses on utilization and thinks this approach is inefficient. This is a fallacy. [You don’t improve the flow of traffic on a highway by improving its utilization (i.e. introducing more vehicles).](https://martinfowler.com/articles/products-over-projects.html)

#### Don’t: Let the team implement a given solution

This is different to a lot of organizations where the business and technology leaders come up with a solution beforehand ("The funnel must be shorter") and let a team implement this. Many organizations using Scrum do this: They are responding to feedback instead of solving problems.

This is not agile even if you have agile teams. See how the team cannot iterate on the solutions anymore but just on a single predefined solution which might even not accepted until the last iteration. This is a waterfall-y process just much less efficient.

![](/assets/Making-sense-of-MVP-1.png)
*Henrik Kniberg (2016) [Source](https://blog.crisp.se/2016/01/25/henrikkniberg/making-sense-of-mvp)*

If you are doing this you should not rewrite your application. It will end up the same like the last one. Instead rewrite small parts of it. And try to change the organizational structure to get more into the best practices outlined above.

## 2. Performance indicators to improve are not clear

> You must have a strict definition and monitoring of performance indicators. If you don’t then the next application version will have similar business problems like the current one.

#### Do: Applications report their status by themselves

All components should have business monitoring capabilities built-in. Too many errors, checkout abandonments or fluctuations during the current time period? The application itself should report this to a monitoring solution.

Like this you have much more context information to what is happening. You need this to always be able to enhance parts of the application where needed.

Your data analysis is much more profound if you have this internal monitoring and changes are seen directly. You allow teams to be creative to improve the given KPIs bit by bit.

#### Don’t: Only do external monitoring

If you don’t have extensive monitoring capabilities built into your application which are adapted to your business case but are only monitoring from the outside with some analytics tool you do not have enough insights to rewrite your application.

It will end with similar shortcomings like the current one. Instead focus on setting clear business goals, carve out parts of your software and organization which optimize this KPI and steadily improve them.

## 3. Architecture Principles

For your problem solving teams to work autonomously and efficiently, your organization needs to commit to architectural principles which are measurable and testable (around ten is a good size). This cuts down on wasted communication around implementation details. Depending on your business these may be different.

#### Do: Make teams adhere to architecture principles

[The art of scalability](http://theartofscalability.com) has a great starting list. Adjust the list to fit your organization. More details can be found in the book (chapter 12), but this should give you a good idea:

- Build small, release fast, fail small
- Design to be monitored
- Design to be disabled
- Design to rollback
- Use mature technologies
- Buy when non-core
- Commodity hardware
- Isolate faults
- N+1 design
- Asynchronous design
- Automation over people
- Horizontal scaling
- Design for multiple live sites
- Stateless systems

Check new solution concepts from your teams against your architecture principles in a small informal architecture board and give suggestions to fulfill them when needed.

#### Don’t: Let a team overlook architecture principles

If each team decides themselves which principles are important you are creating a lot of communication deciding about _who_ and _how_ something should be built instead of _what_ should be built. While the former is inefficient, error-prone and may lead to intra-team fights the latter may be very productive.

Instead build up architecture principles together with your teams to which each one adheres to.

## Conclusion

Teams should solve business problems, need performance indicators and architectural principles. If you don’t have them don’t even think about a rewrite.

If you do, read on why you still should not rewrite and instead shape the organization and contingent software to improve over time:

- [Things You Should Never Do, Part I, Joel Spolsky, 2000](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/)
- [The Big Rewrite, Chad Fowler, 2006](http://chadfowler.com/2006/12/27/the-big-rewrite.html)
- [The Art of Scalability, Michael T. Fisher, Martin L. Abbott, 2015](http://theartofscalability.com)
- [Making sense of MVP (Minimum Viable Product) – and why I prefer Earliest Testable/Usable/Lovable, Henrik Kniberg, 2016](https://blog.crisp.se/2016/01/25/henrikkniberg/making-sense-of-mvp)
- [Products Over Projects, Sriram Narayan, 2017](https://martinfowler.com/articles/products-over-projects.html)

![](/assets/never-demolish-3.jpg)
*Before and after augmenting a building using its main structure, Image © [Philippe Ruault](mailto:philippe.ruault@numericable.fr) - For all uses, thank you to contact him*

(Edit 2018-03-07: Added civil engineering analogy and changed title from "rewrite" to "demolish" to make the intent clearer.)
