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
                if (isSpare(pins))  return WaitingForFirstRollWith1Bonus{};
                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

I wonder if I can move all the state returning logic into base class, which would certainly clean these two classes up. To make them exactly similar, let's write a test for when it's not a spare in the ```WaitingForSecondRollWith1Bonus``` class but it is the last frame.
That means something like this:
```cpp
	{"a strike in the penultimate frame, followed by 2 rolls whose sum < 10", []() {
		Game game;
		RollMany(game, 18, 0);
		game.Roll(10); // strike
		game.Roll(1);
		game.Roll(2);
		Assert::AreEqual(13, game.Score()); // game over here.
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
```

Write just enough code to pass this and all the other tests, and then click [next](Step25.html).
