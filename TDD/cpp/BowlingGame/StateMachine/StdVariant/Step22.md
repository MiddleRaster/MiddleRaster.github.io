---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 22"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step22.html
---

We can move all the utility methods associated with the second roll into a base class, ```SecondRollUtils```, like this:

```cpp
        class SecondRollUtils
        {
            int first;
        public:
            SecondRollUtils(int first) : first(first) {}
            int sumOfRolls(int pins) const { return first + pins; }
            bool isSpare  (int pins) const { return sumOfRolls(pins) == 10; }
        };
```
And then use it in our two ```WaitingForSecondRollWith...``` classes:
```cpp
        struct WaitingForSecondRollWith0Bonuses : private SecondRollUtils
        {
            WaitingForSecondRollWith0Bonuses(int first) : SecondRollUtils(first) {}
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
  ...
        struct WaitingForSecondRollWith1Bonus : SecondRollUtils
        {
            WaitingForSecondRollWith1Bonus(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                ++frame;
                if (isSpare(pins))  return WaitingForFirstRollWith1Bonus{};
                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

That's worked out nicely and made the two classes simpler to read. I see though that the former still has a couple of things that the latter doesn't:
1. I'm not checking that the sum of the two rolls is within range.
2. I'm not ending the game, yet, if necessary.
3. I'm not checking to see if the second roll is a spare AND it's the 10th frame.

Let's fix the first with a quick test:
```cpp
	{"a strike followed by 2 rolls whose sum > 10 throws an exception", []() {
		Assert::ExpectingException<std::invalid_argument>([]()
		{
			Game game;
			game.Roll(10); // strike
			game.Roll(5);
			game.Roll(6); // too big!
		});
	}},
```

Let's write just enough code to pass that and all the other tests, and then click [next](Step23.html).
