---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 19"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step19.html
---

The refactoring to remove the duplication of ```(sumOfRolls(pins) == 10)``` looks like this:

```cpp
        class WaitingForSecondRollWith0Bonuses
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
            bool isSpare  (int pins) const { return sumOfRolls(pins) == 10; }
        public:
            WaitingForSecondRollWith0Bonuses(int first) : first(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins;
                ++frame;
                if ((frame == 10) && isSpare(pins)) return LastFrameWith1Bonus{};
                if (frame >= 10)                    return GameOver{};
                if (isSpare(pins))                  return WaitingForFirstRollWith1Bonus{};
                                                    return WaitingForFirstRollWith0Bonuses {};
            }
        };        
```

Nothing else to refactor that I can see. 

On to the next test:  a single strike followed by two rolls that sum to less than 10, say 1 and 2. The score will be 16. Like so:

```cpp
  ...
	{"1 strike and bonus rolls of 1 and 2 means score is 16", []() {
		Game game;
		game.Roll(10); // strike!
		game.Roll(1);
		game.Roll(2);
		RollMany(game, 16, 0);
		Assert::AreEqual(16, game.Score());
	}},
```

Write just enough code to pass this test and all the others and then click  [next](Step20.html).
