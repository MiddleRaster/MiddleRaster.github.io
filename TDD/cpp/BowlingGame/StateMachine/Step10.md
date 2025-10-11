---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 10"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step10.html
---

Now, some people are under the misimpression that you turn your brain off when doing TDD and never do any design. But that would be foolish.  Let's think hard about the states, the transition, the data-flow.

Here's what I think:  there are really only two state.  Either we're waiting for the bowler to roll his first throw or (if it's not a strike) we're waiting for his second throw.

We could call it something like
- WaitingForFirstRoll, WaitingForSecondRoll
- OnFirstRoll, OnSecondRoll
- RolledOne, RolledTwo
- etc.
I like the first one, personally, but C# people might like the second option as it's similar to C#'s event-driven model.  You can use any names you want, but I'll stick to my choice from here on in.

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

I'll leave it up to you, as to which way you'd like to go.

Select either
[std::unique_ptr way](UniquePtr/Step12.html) or
   [std::variant way](Variant/Step12.html).
