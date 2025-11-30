---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 27"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step27.html
---

All the bonus logic can indeed be moved to the base class:
:
```cpp
        class SecondRollUtils
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
            bool isSpare  (int pins) const { return sumOfRolls(pins) == 10; }
        public:
            SecondRollUtils  (int first) : first(first) {}
            void validateRolls(int pins) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
            }
            State nextState(int pins, int frame) const
            {
                if ((frame >= 10) && isSpare(pins)) return LastFrameWith1Bonus{};
                if (frame >= 10)                    return GameOver{};
                if (isSpare(pins))                  return WaitingForFirstRollWith1Bonus{};
                                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

And then our two derived classes look like this:
```cpp
        struct WaitingForSecondRollWith0Bonuses : private SecondRollUtils
        {
            WaitingForSecondRollWith0Bonuses(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                validateRolls(pins);
                score += pins;
                return nextState(pins, ++frame);
            }
        };
  ...
        struct WaitingForSecondRollWith1Bonus : SecondRollUtils
        {
            WaitingForSecondRollWith1Bonus(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                validateRolls(pins);
                score += pins*2;
                return nextState(pins, ++frame);
            }
        };
```

They are now identical, except for where ```score``` is updated. In one case, there are 0 bonus points added in, and in the other there are 1 set.  We can extract a method with a parameter that specifies what multiple of ```pins``` to sum in.
Let's do that refactoring and clean it up some more, and then click [next](Step28.html).
