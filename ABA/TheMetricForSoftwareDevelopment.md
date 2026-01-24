---
layout: page_with_comments
title: "The Metric for Software Development"
permalink: /ABA/TheMetricForSoftwareDevelopment.html
---

What do we want? High quality code.

When do we want it? As soon as possible.

How can we tell whether we are achieving both of these lofty goals?

What we need is the perfect metric for software development.

- Most people think such a thing doesn't exist.
- Others think all metrics can be gamed and are therefore useless or worse than useless.
- Finally, some think that the ultimate metric is how much money the business makes after building the software.

Let me address the last bullet point first:  the only problem with this "ultimate" metric is that it's a ***trailing indicator***, that is, one that tells you after the fact whether you succeeded or not. 
You probably know if you're out of business, but that knowledge came too late.

This isn't what we want:  we want a ***leading indicator*** that will tell us in advance if we're on track to succeed, and if not, that will allow us to adjust our course as needed.

For the other two bullet points, there is a lot of truth to these:  gameable metrics are worse than no metrics at all. 
In fact, that's the conclusion reached by the excellent book, [Measuring and Managing Performance in Organizations](https://www.amazon.com/Measuring-Managing-Performance-Organizations-Robert/dp/0932633366), by Robert D. Austin.
He also polls 20 "software industry luminaries" asking them their opinions of the ideal metric, and most of them said there probably was one, but they didn't know what it was.

So gather 'round and I'll tell you the perfect metric. Of course, I can't just come out and tell you, because you'll reject it out-of-hand. You've got to listen to the whole spiel.


## Characteristics of a Good Codebase

Much as been written on the topic of what good code looks like (e.g., the SOLID principles, the McCabe metrics, cyclomatic complexity metrics, etc.) and while there may be value in these, I think they all miss the point a little.

Here's my definition:  **a good codebase is one in which it's quick and easy to fix bugs**.

If this doesn't make sense to you, stop now and think about it some more until it does.
You can only get into and stay in a mode where bugs are quick and easy to fix if the architecture is good, clean and simple; if the code is clear and easy to understand; if the code is not coupled ([all DAGs](/DAG/HeaderOnlyNoForwardDeclarations.html)); if the codebase has all those qualities (SOLID, McCabe, cyclomatic copmlexity, etc.) that we want.

You can tell if you're not in such a codebase if there are files you're afraid to touch; if a little change here breaks something way over there; if technical debt has accumulated; if there are band-aids all over the place; if the architecture is hard to see.

But if you do have a "good" codebase, then the following will be true:
1. if it's quick and easy to fix bugs, most, if not all, of them have been fixed already, since it's easy to do so. So you have a high quality product.
2. if it's quick and easy to fix bugs, it's easy to add new features, too. So, you have high team productivity as well.

## Measuring a Good Codebase

To tell if you have a good codebase, all you need to do is measure how quick and easy it is to fix bugs, and the way to do that is to measure the average age of open bugs (ABA), a.k.a., "bug cycle time."

Note that you don't measure how long the closed bugs took to close, but rather you measure how long the currently open bugs have been open.

The really good news is that you're probably collecting all the data you need to calculate the ABA of your codebase.
You take all currently open bugs, subtract their open dates from today's date, and take the average.

Note: a slightly more accurate thing to do is measure the total bug-days of open bugs, but I've found that people don't have as visceral an understanding of bug-days as they do of average bug age.

One nice things to do is to plot that ABA over time to see if your codebase is getting better or worse.
If you do that, you may be struck by how relentless time is:  every day, any untouched open bugs age by one more day.
You can even mine your bug database for your historical ABAs and plot those over time.

A third note: if you open a whole pile of new bugs, you'll see a little dip in the ABA, but time, being relentless, will quickly reassert itself and the ABA starts going up again, and this time it's worse, since there are more bugs.

The only way to keep your ABA nice and low is to fix or close every single bug, and as with many things, there's a right way and a wrong way.

## Bug Policy

The wrong way to keep your ABA low is to have everyone chip in and fix everyone's bugs.

The right way is to have every developer fix his own bugs first before he's allowed to write anything else.

The difference between these two approaches is stark.
The right way has the following consequences:
1. The amount of damage a bug-prone developer can do is limited, because he's spending most of his time fixing up his own errors.
2. On the other hand, the good (bug-free) devs write the vast majoritiy of the code.
3. Buggy devs have incentive to learn now to write bug-free code, because no one likes being in bug-fixing mode all the time.

None of these effects happen if everyone chips in. And that's a real shame, because everywhere I've been, people do it the wrong way.

I rate developers on two axes, speed and buggy-ness, which makes four kinds of devs:

|              |   slow          |   fast         |
|:------------:|:---------------:|:--------------:|
| **bug-free** |   II            |        I       |
| **buggy**    |  III            |       IV       |

The quadrant I devs are the perfect devs, fast and bug-free. 
Every single developer I know thinks he (or she (I'll use "he" through as it's less typing)) is one of these, which is laughable, considering how many bugs there are in the world.
But if you really have one of these on the team, that's great.

Quadrant II devs are slower, but still bug-free.
I like these guys a lot; they're solid, senior devs who are fire-and-forget:  you give them something to do and you know it'll get done with high quality.
And once you measure his velocity, he's easy to schedule.

Quadrant III devs are slow and buggy.
They just get fired, so there's not much to say about them.

But a quadrant IV dev (fast and buggy) is a real problem.
If you let him loose, he'll write a ton of code, and it'll be full of bugs.
And he'll work really hard to try to fix those bugs, but will introduce more.
You've got to find a way to slow that guy down, which is what my bug policy does.

It's unfortunate that the vast majority of dev managers don't get this right.
For example, suppose a team is on a death march working really hard, and the dev mgr starts asking them to stay late, bringing in dinners for them, and to work weekends.

The fast buggy guy is there, working late and on weekends: he takes on more work than he can really handle, works really hard, trying to fix his bugs - very visible.

But the slow bug-free dev takes on a reasonable amount of work and gets it done with little drama - not very visible.

Finally, they ship. And then it's review time. Who gets the promo?
Often it's the fast buggy guy, because he worked so hard.

The problem is managers are confusing hard work (which is easy to see) with good work (which is not easy to see).
My bug policy nips this problem in the bud.

## How to Do It

Once at *StupendousCorp*, I was called in to help a failing team and so became their lead.
Initially, I didn't make any huge waves, but we were missing our deadlines, our incoming bug-rate was through the roof, and I spent one or two days a week working until the wee hours of the morning, fixing up everyone else's bugs.
I hated it.

Eventually, I said, "That's it! If you have a bug, you are required to drop what you're doing and fix it first, before you're allowed to do anything else."

And everything changed. 
The fast buggy devs were now slow (and still buggy) devs, but at least they weren't writing a ton of bugs any more.
The best dev (me) wrote almost all the code, and since I was very experienced with TDD, I was fast and bug-free.
As a team, we were about 5x faster than before.

And I wasn't the only one who saw this; my manager starting doing similarly on all her teams.

So our process became:
1. When a tester found a bug, he would physically go, bug in hand, to the dev who wrote it.
2. That dev would then drop what he was doing and they'd fix the bug together.

There are some nuances. 
A few months after I left that team to become a dev-instructor at *StupendousCorp*, I asked my old manager if she was still enforcing the "fix all bugs first before doing anything else" idea.
She said "Not quite," because her teams were now working on some web-based something-or-other, and all the different browsers implemented their rendering engines slightly differently, so they ended up with a ton of "off-by-1-pixel" bugs.
She decided they weren't worth fixing and closed them.

So the process became "if a bug is found, it's either fixed immediately or closed." No exceptions.

## Effects

I already mentioned the speed-up my team got.

Here's another thing that happened:  the testers wanted to stop filing bugs.

The senior tester told me it took too long, and that makes sense:  why type up the whole bug report into the bug database, and then go to the dev and watch him read it?
He might misunderstand the bug report anyway, but with the tester physically going over to the dev and talking to him about the bug, the possibility of misunderstanding is removed.
So we stopped using a bug database for internally found bugs (though we still had one for externally found bugs).

That surprised me, because testers typically want to keep every last thing in the bug database.
I got pushback from (2nd-level+) test managers when I told them we stopped using a bug database and when I asked why, they said that they needed to know where the buggy areas of the codebase were, so they could provide feedback to the buggy devs.
I'm doubtful that their feedback had any effect whatsoever, especially if that feedback came late in the product cycle, whereas with my way, it had an immediate effect.

As to externally found bugs, ones found by customers or other teams/groups/divisions, we still used a bug database for those.
In the past, the QA lead, the dev lead and the project manager would get in a room for an hour a day, every day, triaging bugs.
With our new approach, the project manager and I would get together and look at any newly found external bugs and we'd categorize them into 3 groups:
1. Not a bug:  sometimes we'd get a bug report from someone who was completely confused about what the product did (though why the person was confused sometimes spurred some thinking). These were closed.
2. An actual bug:  our bug policy kicked in, and the bug was assigned to the dev who owned that area. If that dev didn't actually write that bug, he would assign it to the dev who did. In either case, the dev reponsible would drop what he was doing and fix the bug.
3. A feature request:  sometimes the bug would actually be found to be a feature request. If accepted by the PO, these would be turned into stories, prioritized and put on our backlog. The bug was closed.

Total elapsed time was usually a couple of minutes, once a week. Often the project manager could do the categorization without my input.

## How to Handle Bugs in Scrum and Agile

The rationale for fixing bugs in Scrum or Agile methodologies is simple:  we are working on our backlog items in priority order.
When a bug is found, it must be in a story that had high priority than whatever story the dev is working on now.
He thought the story was done, but it's clearly not, because of this bug that has been found.
So fix the bug in this higher priority story.

There is just terrible advice on the web about this, such as put the bugs on the product backlog and let the Product Owner prioritize them.
The problem here is that this gives the PO a mechanism to de-prioritize bugs, explicitly to drop quality (supposedly so the team can "go faster").
But the PO undoubtedly doesn't realize that technical debt will make the team much slower.

So, don't do it: don't put bugs on the backlog. Period. Just fix them immediately or close them.

## How to Handle Bugs in Kanban

The same rationale (that the bug is in a higher priority story) works with Kanban, too, but even more so:  in Kanban/Lean, there is a concept of an "Andon cord."
It was invented in the Toyota Production System (TPS) and any worker who found a problem was able to pull a cord which stopped the line, until the problem was fixed.

Kanban is all about limiting WIP, swarming on bottlenecks and flow metrics.
The same bad advice given to Scrum/Agile teams about putting bugs on the backlog is given to Kanban teams, too:  don't do it.
Don't put a new card for the bug on the backlog; don't make an expedite swimlane.

The right way is to pull the Andon cord, which stops any upstream work. 
Those workers (usually devs, but it could be UX too if it's a UX bug) swarm on the bottleneck.
The Kanban card stays put (rather than flowing backwards), and no new card is created.
This mechanism [minimizes cycle time and increases throughput](Kanban/KanbanThoughts.html), whereas the other approaches have the opposite effect.

## Should ABA Become a Target?

[Goodhart's Law](https://en.wikipedia.org/wiki/Goodhart%27s_law) says no.

I haven't found a way to game the metric, but I don't doubt that some creative person might come up with one, so I wouldn't use ABA to measure individuals against each other. 
And it makes no sense at all if the ABA is down to less than a hour (or maybe a day).

On the other hand, I think ranking *managers* based on their teams' ABAs is entirely appropriate; not just teams, but groups and divisions, too.
In fact, I'd make ABA part of the calculation of managers' reviews, right up to the CTO's direct reports.
Note that I haven't ever done this, being a peon at *StupedousCorp*, but I did get recognition from two CVPs for my little team as being "best run team in the division."

## Summary

I thought I'd end with a story, one I'd often trot out when I was talking to Principal-level engineering managers at *StupendousCorp*.

I gave a talk on exactly this topic to such a manager, let's call him "Ed," whose team was working on some peta-scale database or other.
They listened politely, asked the right questions, and then I heard nothing.

A while later, maybe a year or so, I happened to notice that Ed's title had changed from Principal Engineering Manager to Partner Engineering Manager (which is actually quite dificult to do at *StupendousCorp*).
So I sent him a little note in email congratulating him on his promo.
He replied with, "It's all because of you" to which I replied, "Huh?"
He said, "Yeah, remember that talk you gave us? When we did that, everything changed."  Nice!

Back to [MiddleRaster's pages](https://middleraster.github.io/).
