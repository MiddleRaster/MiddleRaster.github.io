---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 29"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step29.html
---

To get the perfect game test to pass we need to add a couple of states, the ```WaitingForFirstRollWith3Bonuses``` and ```LastFrameWith2Bonuses``` states, like so:
```cpp
        struct WaitingForFirstRollWith2Bonuses
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                if (pins == 10) {
                    ++frame;
                    return WaitingForFirstRollWith3Bonuses{};
                }
                return WaitingForSecondRollWith1Bonus{pins};
            }
        };
        struct WaitingForFirstRollWith3Bonuses
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*3;
                if (pins == 10) {
                    ++frame;
                    if (frame == 10)
                        return LastFrameWith2Bonuses{};
                    return WaitingForFirstRollWith3Bonuses{};
                }
                return WaitingForSecondRollWith1Bonus{pins};
            }
        };
        struct LastFrameWith1Bonus
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins;
                return GameOver{};
            }
        };
        struct LastFrameWith2Bonuses
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins*2;
                if (pins == 10)
                    return LastFrameWith1Bonus{};
                return GameOver{};
            }
        };
```

Now, I see ```if (pins == 10)``` multiple times in the code, and that could be replaced with a well-named method in an extracted base class.
Let's do that refactoring and then click [next](Step30.html).
