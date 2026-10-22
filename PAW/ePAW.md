---
layout: page_with_comments
title: "The Enhanced Portfolio Alignment Wall"
permalink: /PAW/ePAW.html
---

## 1. The Monday Surprise

Image this thought experiment, which I call **"the Monday Surprise"**:  

Your CEO says to the company, "I'm not going to tell you when we're going to ship. I'll just come in on a Monday of my choice and say, 'We have enough for a product. We're shipping on Friday. Anyone who cannot ship will be fired.' Arrange your work to make this happen."  

What would you do? How would you arrange your work?

If you're just a single team, it's not too hard:  just make sure you're in a shippable state at all times. That means having close to no known bugs at all times, what we used to call ZBB ("Zero Bug Bounce") at *StupendousCorp*.  

But what if your company is 100s of teams, thousands of engineers?  
There are several ways of "scaling up Scrum" and I hate them all - they put more and rigid process into place, require lots of coaching and have spawned yet another cottage industry of certifications, require hiring for newly created roles, etc.

What I want is a light-weight mechanism to coordinate work, one that gives me the flexibility to work as I choose:  Agile, Scrum, Lean, Kanban, XP, etc. with iteration lengths of my choice, etc.


## 2. Bug Policy

The notion of staying at close to zero bugs at all times is a good one. For each team, keep an eye on their bugs counts.  

But you must also keep an eye on technical debt.  If you haven't already, read my post on [the Average Age of Open Bugs (ABA)](/ABA/TheMetricForSoftwareDevelopment.html).  
If you have no technical debt, have great, flexible architecture, if you have all-DAG code, then fixing bugs is quick and easy, and most of them are fixed already, since it's easy to do so.  
If you're in this state, you have little technical debt and that's what ABA measures.

One detail that almost every organization gets wrong:  it's **crucial** that developers fix their own bugs first, before being allowed to write any new code.  See [this section in the aforementioned post](ABA/TheMetricForSoftwareDevelopment.html#bug-policy) for a discussion of the 4 kinds of developers.  

If you get this bug policy right, the team will speed up dramatically. If you get it wrong, it won't.


