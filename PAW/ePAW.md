---
layout: page_with_comments
title: "The Enhanced Portfolio Alignment Wall"
permalink: /PAW/ePAW.html
---

Things execs and managers want to know:  
1. Have we built enough features that we could release a product?
2. Is quality high enough to ship?
3. Is one team the “long pole” that’s holding everybody up?
4. Are there any cross-team dependency commitments that will not be met?


## A Thought Experiment

Image this thought experiment, which I call **"the Monday Surprise"**:  

Your CEO says to the company, "I'm not going to tell you when we're going to ship. I'll just come in on a Monday of my choice and say, 'We have enough for a product. We're shipping on Friday. Anyone who cannot ship will be fired.' Arrange your work to make this happen."  

What would you do? How would you arrange your work?

If you're just a single team, it's not too hard:  just make sure you're in a shippable state at all times. That means having close to no known bugs all the time, what we used to call ZBB ("Zero Bug Bounce") at *StupendousCorp*.  

But what if your company is hundreds of teams, with thousands of engineers?
There are several ways of "scaling up Scrum" but I have found most large-scale Scrum frameworks to be process-heavy and expensive:
They put more process and *rigid* process in place, require lots of coaches, have spawned yet another cottage industry of certifications, require hiring for newly created roles, etc.
They try to plan and coordinate work at a larger scale, do so but in a heavy-handed way.
A pilot rollout costs millions of dollars and success is not guaranteed (or likely, even measured).

What I want is a light-weight mechanism to coordinate work, one that gives me the flexibility to work as I choose:  Agile, Scrum, Lean, Kanban, XP, etc. with iteration lengths of my choice.


## Bug Policy

The notion of staying at close to zero bugs at all times is a good one. For each team, keep an eye on their bugs counts.  

But you must also keep an eye on technical debt.  If you haven't already, read my post on [the Average Age of Open Bugs (ABA)](/ABA/TheMetricForSoftwareDevelopment.html).  
If you have no technical debt, have great, flexible architecture, if you have all-DAG code, then fixing bugs is quick and easy, and most of them are fixed already, since it's easy to do so.  
If you're in this state, you have little technical debt and that's what ABA measures.

One detail that almost every organization gets wrong:  it's **crucial** that developers fix their **own** bugs first, before being allowed to write any new code.  See [this section in the aforementioned post](/ABA/TheMetricForSoftwareDevelopment.html#bug-policy) for a discussion of the 4 kinds of developers.  

If you get this bug policy right, the team will speed up *dramatically*.  
If, instead, you have everyone chip in on bugs, you won't see a speed up at all; rather everyone will be subsidizing one or more bug-prone developers, who will now have no incentive to learn how to become better devs.



## Enhanced Portfolio Alignment Wall

A Portfolio Alignment Wall (PAW) is a well-known but underutilized technique for visualizing work.
However, it suffers from a more-or-less fatal flaw:  it relies on people’s estimates or judgments as to where the work is or will be in the future.  

Wouldn't it be awesome if we could predict when each story will be completed so that we can tell if our plans are any good, or if we have dependency problems, or if one team is "long pole" (that is, the team that's holding everybody up)?  

We have just such a technique, and if you haven't yet, you can read about it here:  [Good Predictions Despite Poor Estimates](/PM/PredictingDespitePoorEstimates.html).  

The key idea is that poor estimating isn't what throws off our predictions of when a story will be done; rather it’s "Second Order Ignorance" (2OI), the "unknown unknowns" that we can't even see because they're completely invisible to us.  

But what we **can** do is track how quickly those 2OI items are showing up (and often they're the most important next thing).  
So we have **two** velocities, two trend lines:  how fast the team is getting work done and how fast the team is discovering new work.  
And here's the important bit:  where those two lines intersect is when you'll ship.

Note that the two lines can be parallel or, worse, diverge; in both of these cases, you'll never ship.

The prediction engine is not estimate-based.
It uses historical completion rates and historical discovery rates.
For the purposes of this article, assume that each story can be assigned a statistically-derived forecast completion date.

Using this technique, I can display when any individual story will be done, and that's the "enhanced" part of the "Enhanced Portfolio Alignment Wall" (ePAW).  It's a read-only, dynamic view of the current state and the predicted state of all backlogs.  
And it’s based on reality: how much work is left, how fast new work is being found and how fast work is being completed; not on people’s guesses or wishful thinking.


The ePAW looks something like this:


{% assign now_s = "now" | date: "%s" %}
{% assign d0    = now_s                | date: "%b %-d" %}
{% assign d1    = now_s | plus: 86400  | date: "%b %-d" %}
{% assign d2    = now_s | plus: 172800 | date: "%b %-d" %}
{% assign d3    = now_s | plus: 259200 | date: "%b %-d" %}
{% assign d4    = now_s | plus: 345600 | date: "%b %-d" %}

|Team Name|ABA (days)| iter n-2 | iter n-1 |...| today {{ d0 }} |  {{ d1 }} | {{ d2 }} | {{ d3 }} | {{ d4 }}  | 
|:-------:|:----:|:-----------:|:-------------:|---|:---:|:----:|:----:|:----:|:----:|
| Team A  |   94 | id1, id2    | id6, id9      |...| id12|      | id20 |      | id31 | 
| Team B  |    7 | id3, id4    | id5, id8      |...|     | id13 |      | id17 | id16 |
| Team C  |   20 | id7         | id10, id11    |...| id18| id19 | id21 |      | id27 |
|    ⋮    |   ⋮   |      ⋮      |       ⋮        |...|  ⋮  |   ⋮  |   ⋮   |   ⋮  |   ⋮  | 


The ePAW solves a real problem:  is one team overloaded?
You can’t tell from story counts or estimates, because some teams slice thinner than others.
But the ePAW looks at completion rates (and discovery rates) in each team’s units and displays predicted completion dates.
Now you have a far more realistic view of which team is the “long pole.” 

So far, the ePAW has answered one of the questions execs and managers need answered.

## Core and NTH

Let’s answer another:  have we built enough to ship?  

To answer this question, we need to distinguish between features that are essential to the next release versus merely “nice to have”.
I call the former, Core (a.k.a, MVP - Minimum Viable Product) and the latter, NTH.  
Each story must be marked as one or the other. Teams are very probably doing this already, via Pri1 (and maybe Pri2 as well), or similar mechanisms.

Then, the ePAW needs a filtered view that shows only the Core items.  
If all the Core stories are to the left of “today,” you have built enough product to ship. Voilà.

So far, we have addressed three of the four questions execs and managers need to have answered. On to the final one.

## Cross-Team Dependencies

Suppose one team, the “consuming” team, needs something from a “supplying” team:  an API, a component, a screen or the code behind a screen, etc.
If the supplying team isn’t ready with the dependency by the time the consuming team needs it, they’re blocked.
And that could cause stalled workers, scheduling delays and so forth.
This is a significant problem because one team’s priorities set by their PO may not match another team’s, set by *their* PO.  

What typically happens? The problem isn’t found until the consuming team starts work on their integration story, finding that there’s nothing ready to integrate yet.
And then after some ad hoc negotiation and possibly escalation, the supplying team promises to deliver at some future date.
This could be unfortunate if the CEO is trying to ship in a hurry.  

If only there were a way to see these cross-team dependency conflicts ahead of time.
If they were identified earlier and addressed, the schedule would not be impacted.  

To make that happen, we need to identify those cards that depend on other cards.
All the backlog management tools I know (Jira, Azure DevOps, etc.) have this capability built-in.
But there are problems:  finding the right story in another team’s backlog isn’t trivial and is a real pain point.
So often, the link to the supplying team’s card is missing entirely, though the consuming team’s card is annotated in some way.  

Let’s assume for now that the dependency links do exist and we’ll address the missing links in just a bit.  


What we need is for the ePAW to display in vivid red if a supplying team’s card is predicted to be done after the consuming team’s card.
In fact, the consuming team needs some time to work on it, so the dependency should be done, say, 1 iteration before the consumer needs it.
If that is not the case, highlight both cards in red and draw a red arrowed line from the consuming card to the supplying card.  


What should teams do if this situation is detected?  Whatever you’re doing now, but it typically includes:  
- raise the priority of the supplying card
- lower the priority of the consuming card
- escalate to management if the POs cannot agree
- loan some team members from the consuming team to the supplying team
- the nuclear option:  temporarily combine the two teams


## Missing Dependency Links

Up until now, we haven’t changed anything about how the teams are working now.
The ePAW is a read-only view into the state and the predicted state of all backlogs.
And the ePAW will always be read-only:  it’s **an execution-monitoring system**. Teams can work however they like: Scrum, Agile, XP, Lean, Kanban, waterfall or cowboy coding.  


To address the missing dependency links, I suggest the following procedures:
1. The consuming team creates their story card just like they do now.
2. If it’s determined that it depends on another team’s work, a card is created **by the consuming team’s PO** ***in the supplying team’s backlog*** and a link is created from one card to the other team’s card.
3. The supplying team’s PO sets the card’s priority and sizing as he sees fit. He’ll probably link it to one of his team’s existing stories.
4. The ePAW will show in red if there’s a conflict and if so, your normal negotiation or escalation procedure kicks in.


## Proof of Concept for CEO/CTO

So we’ve got a notion of what to build that answers the four questions that execs and managers need answered in order to ship at a large scale.  
Is an ePAW plus a proper bug policy (checked by ABA) enough to replace something like SAFe? 

Maybe; it depends on what your org’s deficits are.
If your org has a planning deficit, then maybe ePAW is not for you, though in my experience, companies can plan all over the place. That’s not usually the problem.  

More often, there is an execution deficit, often around quality, or a visibility problem.
In those cases ePAW can very likely help.  

So what should a CEO or CTO do?
If I were the CEO, I’d run a pilot, a trial of ePAW and that means building a proof of concept.
How long will that take?
I estimate a senior or principal dev, working for the CEO/CTO/Chief of Staff, could create a PoC in a week or two.  

Here are the pieces for, say, a CLI app:
1. Ingestion of the work backlog and bug database
    1. this is very likely the hardest part,
    2. but as a first cut, you could manually export from your backlog management tool and your bug database as CSV, xml/json, etc., files and read them in.
    3. Do it programmatically later
2. The ABA calculation engine
3. The CFD/burn-up calculator to come up with a predicted completion date for each unfinished story
4. The filtering of Core vs NTH
    1. This could be done at the export level, above, initially.
    2. Later, make it a command-line argument and do the display filtering yourself
5. Output as:
    1. first as text, piped into a file, viewed in notepad
    2. or perhaps markdown; view in an appropriate viewer (e.g., Visual Studio)
    3. or CSV and view in Excel
    4. and finally, json or xml, to be consumed by a web page
6. Write the webpage that consumes the generated output from the CLI app


Initially, I would privately watch how the organization executes.
- What are the teams’ ABAs and are they getting worse?
- Is there an obviously overloaded team?
- Are they working on NTH items while Core items remain unfinished?
- Are there any as yet unobserved (and unannounced and unaddressed) dependency conflicts?

If the answers to all these questions are No and you decide to abandon ePAW, the cost of failure is a dev salary for a week or two.

If on the other hand, you see problems, I would recommend the following:
1. Collect baseline data for as long as you see fit before making any policy changes
2. Implement a proper bug policy where each developer is required to fix immediately (or close immediately) any of his own bugs before he is allowed to write new code.
    1. You should see the ABA and bug counts drop significantly.
    2. But more interestingly, you should see the **density** of completed stories increase (to the left of “today”).
3. Implement the missing dependency link process (piloted at first with just a few select teams)
    1. You’ll initially probably see lots of red, hopefully way ahead of time where the issues can be addressed early before they cause scheduling problems
    2. You’ll want to display the automatically updating web page prominently somewhere (“an information radiator”) and to give out the URL so anyone can see the ePAW

The value of an ePAW pilot compared to its cost is compelling to any CEO/CTO, in my opinion, especially when compared to a pilot of SAFe or similar frameworks.

## Conclusion 

Traditional portfolio management attempts to improve execution through more planning.
ePAW instead focuses on visibility into execution itself.
If teams maintain quality through low ABA, forecast completion dates through observed throughput and discovery rates, and expose cross-team dependency conflicts early, executives gain a real-time view of release readiness without imposing additional process on engineering teams.


