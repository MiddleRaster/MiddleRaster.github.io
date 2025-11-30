---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 26"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step26.html
---

Passing that last test makes our ```WaitingForSecondRollWith1Bonus``` class look exactly like the ```WaitingForSecondRollWith0Bonuses``` class:
```cpp
        struct WaitingForSecondRollWith1Bonus : SecondRollUtils
        {
            WaitingForSecondRollWith1Bonus(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                validateRolls(pins);
                score += pins*2;
                ++frame;
                if ((frame >= 10) && isSpare(pins)) return LastFrameWith1Bonus{};
                if (frame >= 10)    return GameOver{};
                if (isSpare(pins))  return WaitingForFirstRollWith1Bonus{};
                                    return WaitingForFirstRollWith0Bonuses{};
            }
```

So now we can extract the bonus logic and put it on the base class. Let's do that and then click [next](Step27.html).
