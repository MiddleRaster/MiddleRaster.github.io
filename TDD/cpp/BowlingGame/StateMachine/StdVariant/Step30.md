---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 30"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step30.html
---

To remove the ```if (pins == 10)``` from multiple places, we can extract a base class, ```FirstRollUtils```, and put a method, ```IsStrike(int pins)``` there:
```cpp
        struct FirstRollUtils
        {
            static bool IsStrike(int pins) { return pins == 10; }
        };
```
And use it several times:
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
  ...
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
  ...
        struct LastFrameWith2Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins*2;
                if (IsStrike(pins))
                    return LastFrameWith1Bonus{};
                return GameOver{};
            }
        };
```
Looks good.

However, I notice that the ```WaitingForFirstRollWith1Bonus``` class does ***not*** derive from ```FirstRollUtils```, and it doesn't in fact check for a strike anywhere. Maybe we're missing a test? Let's try it by writing:
```cpp
	{"a spare followed by a strike and a two bonus rolls of 1 and 2 means score is ", []() {
		Game game;
		game.Roll(5);
		game.Roll(5); // spare
		game.Roll(10); // strike
		game.Roll(1);
		game.Roll(2);
		RollMany(game, 14, 0);
		Assert::AreEqual(36, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
```
And when run, this test fails, throwing an exception with message, ```sum of rolls in a frame must be <= 10```. Clearly, we need that ```IsStrike``` check.
Let's do that and then click [next](Step31.html).
