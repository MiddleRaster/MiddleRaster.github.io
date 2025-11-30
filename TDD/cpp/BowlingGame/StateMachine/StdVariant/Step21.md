---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 21"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step21.html
---

To make the "single strike, single spare and a bonus roll of 1" test work, we need to add a line similar to ```WaitingForSecondRollWith0Bonuses```:
```cpp
        class WaitingForSecondRollWith1Bonus
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
            bool isSpare  (int pins) const { return sumOfRolls(pins) == 10; }
        public:
            WaitingForSecondRollWith1Bonus(int first) : first(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                ++frame;
                if (frame >= 10)    return GameOver{};
                if (isSpare(pins))  return WaitingForFirstRollWith1Bonus{};
                                    return WaitingForFirstRollWith0Bonuses{};            }
        };
```
I added the check for ```isSpare(pins)``` and if it's true, we return to ```WaitingForFirstRollWith1Bonus```. 

I see a ton of commonality with the ```WaitingForSecondRollWith0Bonuses``` class. 
Let's extract a base class, derive both of these classes from it, and then click [next](Step22.html).
