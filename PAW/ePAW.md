---
layout: page_with_comments
title: "The Enhanced Portfolio Alignment Wall"
permalink: /PAW/ePAW.html
---

## The Monday Surprise

Image this thought experiment, which I call **"the Monday Surprise"**:  

Your CEO says to the company, "I'm not going to tell you when we're going to ship. I'll just come in on a Monday of my choice and say, 'We have enough for a product. We're shipping on Friday. Anyone who cannot ship will be fired.' Arrange your work to make this happen."  

What would you do? How would you arrange your work?

If you're just a single team, it's not too hard:  just make sure you're in a shippable state at all times. That means having close to no known bugs at all times, what we used to call ZBB ("Zero Bug Bounce") at *StupendousCorp*.  

But what if your company is 100s of teams, thousands of engineers?  
There are several ways of "scaling up Scrum" and I hate them all - they put more and rigid process into place, require lots of coaching and have spawned yet another cottage industry of certifications, require hiring for newly created roles, etc.

What I want is a light-weight mechanism to coordinate work, one that gives me the flexibility to work as I choose:  Agile, Scrum, Lean, Kanban, XP, etc. with iteration lengths of my choice, etc.


## Bug Policy

The notion of staying at close to zero bugs at all times is a good one. For each team, keep an eye on their bugs counts.  

But you must also keep an eye on technical debt.  If you haven't already, read my post on [the Average Age of Open Bugs (ABA)](/ABA/TheMetricForSoftwareDevelopment.html).  
If you have no technical debt, have great, flexible architecture, if you have all-DAG code, then fixing bugs is quick and easy, and most of them are fixed already, since it's easy to do so.  
If you're in this state, you have little technical debt and that's what ABA measures.

One detail that almost every organization gets wrong:  it's **crucial** that developers fix their own bugs first, before being allowed to write any new code.  See [this section in the aforementioned post](/ABA/TheMetricForSoftwareDevelopment.html#bug-policy) for a discussion of the 4 kinds of developers.  

If you get this bug policy right, the team will speed up *dramatically*.  
If, instead, you have everyone chip in on bugs, you won't see a speed up at all; rather everyone will be subsidizing one or more bug-prone developers, who will now have no incentive to learn how to become better devs.



## Enhanced Portfolio Alignment Wall

A Portfolio Alignment Wall (PAW) is a well-known but underutilized technique for visualizing work.  
However, if suffers from a more-or-less fatal flaw:  it relies on peoples estimates or judgments to where the work is or will be in the future.  

Wouldn't it be awesome if we could predict when each story will be completed so that we can tell if our plans are any good, or if we have dependency problems, or if one team is "long pole" (that is, the team that's hold everybody up)?  

We have just such a technique, and if you haven't you can read about it here:  [Good Predictions Despite Poor Estimates](/PM/PredictingDespitePoorEstimates.html).

The key idea is that estimating isn't the thing that throws off our predictions of when a story will be done; rather is "Second Order Ignorance" (2OI), the "unknown unknowns" that we can't even see because they're completely invisible to us.  

But what we **can** do is track how quickly those 2OI items are showing up (and often they're the most important next thing).  
So we have **two** velocities:  how fast the team is getting work done and how fast the team is discovering new work.  
And here's the important bit:  where those two lines intersect is when you'll ship.

Note that the two lines can be parallel or worse, diverge; in both of these cases, you'll never ship.

Using the same technique, I can tell when **any** individual story will be done, and that's the "enhanced" part of the "Enhanced Portfolio Alignment Wall" (ePAW).  It's a read-only dynamic view of the current state and the predicted state of all backlogs.

The ePAW looks like this:



|Team Name|ABA (days)| done week n | done week n-1 |...|Today|  1/1 |  1/2 | 1/3  | 1/4  | 
|---------|:--------:|-------------|---------------|---|-----|:----:|:----:|:----:|:----:|
| Team A  |     94   | id1, id2    | id6, id9      |...| id12|      | id20 |      | id31 | 
| Team B  |      7   | id3, id4    | id5, id8      |...|     | id13 |      | id17 | id16 |
| Tean C  |     20   | id7         | id10, id11    |...| id18| id19 | id21 |      | id27 | 





