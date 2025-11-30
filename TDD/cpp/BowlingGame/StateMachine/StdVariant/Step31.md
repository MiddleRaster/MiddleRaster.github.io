---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 31"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step31.html
---

Adding that missing code is easy:
```cpp
        struct WaitingForFirstRollWith1Bonus : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                if (IsStrike(pins)) {
                    ++frame;
                    return WaitingForFirstRollWith2Bonuses{};
                }
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };
```


Now, looking at all the first rolls states, something doesn't look right to me:
```cpp
        struct WaitingForFirstRollWith0Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins;
                if (IsStrike(pins)) {
                    ++frame;
                    return WaitingForFirstRollWith2Bonuses{};
                }
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };
        struct WaitingForFirstRollWith1Bonus : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                if (IsStrike(pins)) {
                    ++frame;
                    return WaitingForFirstRollWith2Bonuses{};
                }
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };
        struct WaitingForFirstRollWith2Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                if (IsStrike(pins)) {
                    ++frame;
                    return WaitingForFirstRollWith3Bonuses{};
                }
                return WaitingForSecondRollWith1Bonus{pins};
            }
        };
        struct WaitingForFirstRollWith3Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*3;
                if (IsStrike(pins)) {
                    ++frame;
                    if (frame == 10)
                        return LastFrameWith2Bonuses{};
                    return WaitingForFirstRollWith3Bonuses{};
                }
                return WaitingForSecondRollWith1Bonus{pins};
            }
        };
```
Only the last one is checking to see if we're in the last frame, which needs to be handled specially. And that means that we're missing (at least) 3 tests.

TODO:  sigh. I'll add them tomorrow.

Let's do that and then click [next](Step32.html).
