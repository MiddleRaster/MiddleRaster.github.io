---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 11"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step11.html
---

Here's the std::unique_ptr state machine way:
```
#pragma once

#ifndef BOWLING_H
#define BOWLING_H

#include <memory>

namespace Bowling
{
    class Game
    {
        class Scorer
        {
            int score = 0;
        public:
            void AddRoll(int pins) { score += pins; }
            int Score() const { return score; }
        };

        struct State
        {
            virtual std::unique_ptr<State> Update(Scorer& /*scorer*/, int /*pins*/) const = 0;
            virtual ~State() {}
        };
        struct WaitingForFirstRoll : public State
        {
            std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
            {
                scorer.AddRoll(pins);
                return std::make_unique<WaitingForSecondRoll>();
            }
        };
        struct WaitingForSecondRoll : public State
        {
            std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
            {
                scorer.AddRoll(pins);
                return std::make_unique<WaitingForFirstRoll>();
            }
        };
        Scorer scorer;
        std::unique_ptr<State> state = std::make_unique<WaitingForFirstRoll>();
    public:
        void Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");
            state = state->Update(scorer, pins);
        }
        int Score() const { return scorer.Score(); }
    };
}
#endif
```

There's an abstract base class, ```State```, which has the basic machinery for a state machine:  an ```Update``` method that returns a ```std::unique_ptr<State>```.
Now, notice that the ```Update``` method is virtual *and const*. Having mutable states in a state machine would be very wrong, so I'm marking it as const.

There are two concrete implementations of that abstract base class and a reference to the ```Scorer``` is passed to the ```Update``` method along with the number of pins knocked down. It then returns a unique pointer to the other concrete class instance.

Looks clean to me; nothing further to refactor. Let's comment in the spares test. It still fails as before.

Write just enough code to pass that and all the other test, and then click  [next](Step12.html).
