---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 20"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step20.html
---

To make the "single strike plus 2 bonus rolls" test pass we need two classes, ```WaitingForFirstRollWith2Bonuses``` for the state after we just rolled the strike, and ```WaitingForSecondRollWith1Bonus``` after the next roll has used up one of the bonus rolls:

For rolling a strike, we need to modify the ```WaitingForFirstRollWith0Bonuses``` state:
```cpp
        struct WaitingForFirstRollWith0Bonuses
        {
            State Update(int pins, int& score, int& frame) const
            {
                score += pins;
                if (pins == 10) {
                    ++frame;
                    return WaitingForFirstRollWith2Bonuses{};
                }
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };
```

And since we now have 2 bonus rolls spending, we need a new state for ```WaitingForFirstRollWith2Bonuses```:
```cpp
        class WaitingForSecondRollWith1Bonus
        {
            int first;
        public:
            WaitingForSecondRollWith1Bonus(int first) : first(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                score += pins*2;
                ++frame;
                return WaitingForFirstRollWith0Bonuses{};
            }
        };
```

Ok, that passes, but a couple of points:
1. I'll bet in the future, I'll end up extracting a base class and using it in both the ```WaitingForSecondRollWith0Bonuses``` and ```WaitingForSecondRollWith1Bonus``` classes, since I expect there'll be a lot of similarity;
2. I'm not ending the game, yet, if necessary.
3. I'm not checking to see if the second roll is a spare.

Lots of things to do. Let's write a test for that last case first:  a strike, followed by a spare, followed by a bonus roll, say 1, and the rest gutterballs.

```cpp
	{"1 strike, 1 spare, a one and the rest gutterballs means score is 32", []() {
		Game game;
		game.Roll(10); // strike!
		game.Roll(5);
		game.Roll(5); // spare
		game.Roll(1);
		RollMany(game, 15, 0);
		Assert::AreEqual(32, game.Score());
	}},
```

Write just enough code to pass this test and all the others and then click  [next](Step21.html).
