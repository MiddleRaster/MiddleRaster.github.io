---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 33"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step33.html
---

Ok, fixing up the ```WaitingForFirstRollWith1Bonus``` state ends up like this:
```cpp
        struct WaitingForFirstRollWith1Bonus : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                if (IsStrike(pins)) {
                    ++frame;
                    if (frame == 10)
                        return LastFrameWith2Bonuses{};
                    return WaitingForFirstRollWith2Bonuses{};
                }
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };
```
So now all the ```WaitingForFirstRollWithXBonuses``` all look similar, so we can refactor the guts of the ```Update``` method into a base class.

Let's do that and then click [next](Step34.html).
