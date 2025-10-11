---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 12"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step12.html
---

Here's the refactored version:
```
    struct State
    {
        int& score;
        State(int& refToScore) : score(refToScore) {}
        void AddToScore(int pins) { score += pins; }

        virtual std::unique_ptr<State> Update(int /*pins*/) = 0;
        virtual ~State() {}
    };
    struct WaitingForFirstRoll : public State
    {
        WaitingForFirstRoll(int& refToScore) : State(refToScore) {}
        std::unique_ptr<State> Update(int pins) override
        {
            AddToScore(pins);
            return std::make_unique<WaitingForSecondRoll>(score);
        }
    };
    struct WaitingForSecondRoll : public State
    {
        WaitingForSecondRoll(int& refToScore) : State(refToScore) {}
        std::unique_ptr<State> Update(int pins) override
        {
            AddToScore(pins);
            return std::make_unique<WaitingForFirstRoll>(score);
        }
    };    
```

The ```State``` class now holds onto the reference to ```score``` and that reference is updated in the ```AddToScore``` method.

I don't like it:  the ```State``` class is doing double-duty:  its reason for being is to provide an interface for the state machine **and** it's updating the score.
That's two things and violates the Single-class/Single-responsibility principle.

Let's refactor some more to fix that, and then click [next](Step13.html).
