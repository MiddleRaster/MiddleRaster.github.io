---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 24"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step24.html
---

After refactoring that precondition into an extracted method, ```validateRolls(int pins)```, our code looks like this:
```cpp
        class SecondRollUtils
        {
            int first;
        public:
            SecondRollUtils(int first) : first(first) {}
            int sumOfRolls    (int pins) const { return first + pins; }
            bool isSpare      (int pins) const { return sumOfRolls(pins) == 10; }
            void validateRolls(int pins) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
            }
        };
  ...
        struct WaitingForSecondRollWith0Bonuses : private SecondRollUtils
        {
            WaitingForSecondRollWith0Bonuses(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                validateRolls(pins);
                score += pins;
                ++frame;
                if ((frame == 10) && isSpare(pins)) return LastFrameWith1Bonus{};
                if (frame >= 10)                    return GameOver{};
                if (isSpare(pins))                  return WaitingForFirstRollWith1Bonus{};
                                                    return WaitingForFirstRollWith0Bonuses {};
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
                ++frame;
                if (frame >= 10)    return GameOver{};
                if (isSpare(pins))  return WaitingForFirstRollWith1Bonus{};
                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

// TODO:  maybe move all the state returning logic into base class?  For now, I'm going back and putting in the precondition tests earlier, to keep in sync with UniquePtr.



and then click [next](Step25/.html).
