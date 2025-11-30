---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 25"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step25.html
---

A non-Spare in the final frame of ```WaitingForSecondRollWith1Bonus``` means the game is over:
:
```cpp
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

And to make the ```WaitingForSecondRollWith1Bonus``` class look exactly like ```WaitingForSecondRollWith0Bonuses``` so we can extract the bonus logic to the base class, we need a test for a spare in the 10th frame, like so:
```cpp
	{"a strike in the penultimate frame, followed by a spare mean we go on to the bonus frame", []() {
		Game game;
		RollMany(game, 18, 0);
		game.Roll(10); // strike
		game.Roll(5);
		game.Roll(5); // spare
		game.Roll(1); // single bonus
		Assert::AreEqual(31, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
```

Write just enough code to pass this and all the other tests, and then click [next](Step26.html).
