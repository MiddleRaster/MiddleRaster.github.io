---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 23"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step23.html
---

Checking that the rolls in a frame sum up to within range looks like this:
```cpp
        struct WaitingForSecondRollWith1Bonus : SecondRollUtils
        {
            WaitingForSecondRollWith1Bonus(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins*2;
                ++frame;
                if (frame >= 10)    return GameOver{};
                if (isSpare(pins))  return WaitingForFirstRollWith1Bonus{};
                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

That precondition is used twice, so let's extract that precondition into its own method and put it on the ```SecondRollUtils``` class, and then click [next](Step24/.html).
