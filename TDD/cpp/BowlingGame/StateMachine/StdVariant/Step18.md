---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 18"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step18.html
---

Making the "all spares" test pass means adding some logic to the ```WaitingForSecondRollWith1Bonus``` class:

```cpp
        class WaitingForSecondRollWith0Bonuses
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
        public:
            WaitingForSecondRollWith0Bonuses(int first) : first(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins;
                ++frame;
                if ((frame == 10) && (sumOfRolls(pins) == 10))  return LastFrameWith1Bonus{};
                if (frame >= 10)                                return GameOver{};
                if (sumOfRolls(pins) == 10)                     return WaitingForFirstRollWith1Bonus{};
                                                                return WaitingForFirstRollWith0Bonuses {};
            }
        };
  ...
        struct LastFrameWith1Bonus
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins;
                return GameOver{};
            }
        };
```

If we rolled a spare in the last frame, we still have 1 roll to go, so we check to see if we're in frame 10 now and if it's a spare, and if both those are true, we go to a special ```LastFrameWith1Bonus``` state,
which simply sums in the pins of the next roll and ends the game.

I see a little refactoring opportunity, though:  two instances of ```(sumOfRolls(pins) == 10)```. 
Let's refactor that and then click  [next](Step19.html).
