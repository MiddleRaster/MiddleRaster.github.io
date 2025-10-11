---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 11"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step11.html
---

Here's the std::unique_ptr state machine way:
```
#ifndef BOWLING_H
#define BOWLING_H

#include <memory>

namespace Bowling
{
    class Game
    {
        struct State
        {
            virtual std::unique_ptr<State> Update(int /*pins*/) = 0;
            virtual ~State() {}
        };
        class WaitingForFirstRoll : public State
        {
            int& score;
        public:
            WaitingForFirstRoll(int& refToScore) : score(refToScore) {}
            std::unique_ptr<State> Update(int pins) override
            {
                score += pins;
                return std::make_unique<WaitingForSecondRoll>(score);
            }
        };
        class WaitingForSecondRoll : public State
        {
            int& score;
        public:
            WaitingForSecondRoll(int& refToScore) : score(refToScore) {}
            std::unique_ptr<State> Update(int pins) override
            {
                score += pins;
                return std::make_unique<WaitingForFirstRoll>(score);
            }
        };

        int score = 0;
        std::unique_ptr<State> state = std::make_unique<WaitingForFirstRoll>(score);

    public:
        void Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");

            state = state->Update(pins);
        }
        int Score() const { return score; }
    };
}
#endif
```

There's an abstract base class, ```State```, which has the basic machinery for a state machine:  an ```Update``` method that returns a ```std::unique_ptr<State>```.

There are two concrete implementations of that abstract base class, each of which holds on to a reference to ```score``` and updates it in the ```Update``` method, before returning a unique pointer to the other concrete class instance.

Plenty of refactoring opportunities:  there's much duplication between the two concrete classes. Let's push a bunch of that into their base class.

When you've done that refactoring, click [next](Step12.html).
