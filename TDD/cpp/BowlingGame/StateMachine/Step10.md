---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 10"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step10.html
---

Now, some people are under the misimpression that you turn your brain off when doing TDD and never do any design. But that would be foolish.  Let's think hard about the states, the transitions, the data-flow.

So, as I see it we need to track two things:  1) whether we're on the first roll of a frame or the second roll, and 2) how many bonus rolls are pending. So there are two possibilities:
we either combine these two requirements into a whole bunch of little states, or we keep them separate, with just a couple of states, but with an additional class that handles all the bonus roll logic.

Option A:

With an additional class that handles all the bonus roll logic, we would only need a couple of state classes.
Either we're waiting for the bowler to roll his first throw or (if it's not a strike) we're waiting for his second throw.

We could call them, for instance:
- WaitingForFirstRoll, WaitingForSecondRoll
- OnFirstRoll, OnSecondRoll
- RolledOne, RolledTwo
- etc.

I like the first one, personally, but C# people might like the second option as it's similar to C#'s event-driven model.  You can use any names you want, but I'll stick to my choice from here on in.


Option B:

The other option is to incorporate the bonus roll logic right into the states. In this case, we would have classes named like:
- WaitingForFirstRollWith0Bonuses
- WaitingForFirstRollWith1Bonus
- WaitingForSecondRollWith0Bonuses
- etc.
We'll end up with a ton of little classes, but hopefully they'll be really simple.

<br><br>

Now, we have another decision to make:  how will we implement the state machine?

Option 1:

I assume you've read the Gang of Four's 1994 classic, "Design Patterns:  Elements of Reusable Object-Oriented Software". 
In that book, the state machine consisted of an abstract base class, with one or more concrete implementations. 
The important part is that there is an ```Update``` (or similarly named) method that does some work and then returns a new instance of one of the concrete implementations; something like this:
```
    State* state = new WaitingForFirstRoll();
    while(!gameIsDone) {
        State* newState = state->Update(...);
        delete state;
        state = newState;
    }
```
By 1994 standards, this was fine, but by today's standards it's pretty ugly.  Raw pointers, raw new/delete. It'd never pass my codereview. But an easy fix is to use std::unique_ptr.

Option 2:

In modern C++, we have other options:  in particular, we could use a std::variant<WaitingForFirstRoll, WaitingForSecondRoll> and a visitor. No new/delete, no pointers.

Discussion:

Both have a well-known circular dependency cycle; that's unavoidable.  WaitingForFirstRoll must be able to create a WaitingForSecondRoll and vice versa.
If performance is an issue, allocating on the heap is probably slower than template magic.

<br>

I'll leave it up to you, as to which way you'd like to go. But I'll implement them both ways:
- std::unique_ptr of 2 states and additional logic class, or
- std::variant and states including bonus states.

Select either
[std::unique_ptr and additional bonus logic class](UniquePtr/Step11.html) or
[std::variant way and states including bonus states](Variant/Step11.html).
