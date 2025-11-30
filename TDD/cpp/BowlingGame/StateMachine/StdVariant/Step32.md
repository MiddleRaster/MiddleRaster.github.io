---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 32"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step32.html
---

Adding the special handling to the ```WaitingForFirstRollWith2Bonuses``` state ends up like this:
```cpp
        struct WaitingForFirstRollWith2Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
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

I also clean up the ```LastFrameWithXBonuses``` states, like this:
```cpp
        struct LastFrameWith1Bonus
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins; 
                return GameOver{};
            }
        };
        struct LastFrameWith2Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins;
                return LastFrameWith1Bonus{};
            }
        };
        struct LastFrameWith3Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins*2;
                return LastFrameWith1Bonus{};
            }
        };
```
Those classes don't need to check for the number of frames or strikes or spares:  they just sum in the bonus values for 1 or 2 rolls.

The ```WaitingForFirstRollWithXBonuses``` classes though do need to check for strikes and being in the last frame.
Let's write a test for the missing check for being in the 10th frame in the ```WaitingForFirstRollWith1Bonus``` state. Something like:
```cpp
	{"16 gutterballs, then a spare, a strike and two bonus rolls of 1 and 2 means score is 33", []() {
		Game game;
		RollMany(game, 16, 0);
		game.Roll(5);
		game.Roll(5); // spare
		game.Roll(10); // strike
		game.Roll(1);
		game.Roll(2);
		Assert::AreEqual(33, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
```

Get all the tests to pass and then click [next](Step33.html).
