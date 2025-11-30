---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 28"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step28.html
---

We can move all the Update logic into the base class if we do the following:
```cpp
        class SecondRollUtils
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
            bool isSpare  (int pins) const { return sumOfRolls(pins) == 10; }
        public:
            SecondRollUtils  (int first) : first(first) {}
            State update(int pins, int& score, int& frame, int bonusMultiplier) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins*(1+bonusMultiplier);
                ++frame;
                if ((frame >= 10) && isSpare(pins)) return LastFrameWith1Bonus{};
                if (frame >= 10)                    return GameOver{};
                if (isSpare(pins))                  return WaitingForFirstRollWith1Bonus{};
                                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

And then the two derived classes look like this:
```cpp
        struct WaitingForSecondRollWith0Bonuses : private SecondRollUtils
        {
            WaitingForSecondRollWith0Bonuses(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const { return update(pins, score, frame, 0); }
        };
        struct WaitingForSecondRollWith1Bonus : SecondRollUtils
        {
            WaitingForSecondRollWith1Bonus(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const { return update(pins, score, frame, 1); }
        };
```
The only difference is the multiplier, the last parameter on the ```update``` method. You'll note that I removed the ```validateRolls``` and ```nextState``` methods and put their bodies inside ```update```, as they were only called once.

Overall, I'm happy with the direction we're going in.  What's left? I think we're getting close. Let's write the perfect game test:
```cpp
	{"perfect game means score is 300", []() {
		Game game;
		RollMany(game, 12, 10);
		Assert::AreEqual(300, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
```

Let's write just enough code to pass this last test and all the rest, andthen click [next](Step29.html).
