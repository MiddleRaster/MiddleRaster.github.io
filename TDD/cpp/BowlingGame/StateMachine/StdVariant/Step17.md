---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 17"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step17.html
---

Adding the ```GameOver``` state is straightforward, too:

```cpp
  ...
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
             if (frame >= 10)            return GameOver{};
             if (sumOfRolls(pins) == 10) return WaitingForFirstRollWith1Bonus{};
                                         return WaitingForFirstRollWith0Bonuses {};
         }
     };
  ...
     struct GameOver
     {
         State Update(int /*pins*/, int& /*score*/, int& /*frame*/) const
         {
             throw std::invalid_argument("cannot keep bowling after game is over");
         }
     };
  ...
```

We add a ```GameOver``` struct that can be reused for all the other ways to end a game, too.

Let's write the next test, which is an "all spares" test:  roll 5 21 times and the score should be 150.
```cpp
VsTest tests[] = {
  ...
	{"all spares means score is 150", []() {
		Game game;
		RollMany(game, 21, 5);
		Assert::AreEqual(150, game.Score());
	}},
};
```

Now, write just enough code to pass that and all the other tests, and then click  [next](Step18.html).
