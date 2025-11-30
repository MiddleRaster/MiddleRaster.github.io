---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 11"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step11.html
---

Now, before we implement anything, let's think hard about what states we actually need. We need a "waiting for first roll when 0 bonus rolls are pending" state, and a similar one for the second roll.
But some combinations of bonus rolls pending aren't possible: e.g., "waiting for second roll with 3 bonus rolls" is impossible, because the previous roll would have used up 2 bonus rolls. Ditto 2 bonus rolls, on the second roll.
We could also use a "game over" state that just throws an exception if it ever gets called.  Let's refactor what we have so far, that is, up to but not including the Spare test to use std::variant.

I've been playing with C++20 modules recently, so while I'm at it, I'm going to switch over to my Tdd20-style unit test harness and run them in Visual Studio's Test Explorer: [here](https://github.com/MiddleRaster/Tdd20.TestAdapter). 
The tests that pass so far look like this:

```cpp
import tdd20;
import VsTdd20;
import std;
using namespace TDD20;

#include "BowlingGame.h"
using namespace Bowling;

void RollMany(Game& game, int rolls, int pins)
{
	for(int i=0; i<rolls; ++i)
		game.Roll(pins);
}

VsTest tests[] = {
	{"score is 0 before any rolls", []() { Game game;
		Assert::AreEqual(0, game.Score());
	}},
	{"all gutterballs means score is 0", []() { Game game;
		RollMany(game, 20, 0);
		Assert::AreEqual(0, game.Score());
	}},
	{"all rolls knocking down 1 pin each time means score is 20", []() { Game game;
		RollMany(game, 20, 1);
		Assert::AreEqual(20, game.Score());
	}},
	{"a roll must be positive", []()
	{
		Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(-1); });
	}},
	{"a roll must be <= 10", []()
	{
		Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(11); });
	}},
	//{"a spare plus two bonus rolls of 1 and 2 and then all gutterballs means score is 16", []() {
	//	Game game;
	//	game.Roll(5);
	//	game.Roll(5); // spare
	//	game.Roll(1);
	//	game.Roll(2);
	//	RollMany(game, 16, 0);
	//	Assert::AreEqual(16, game.Score());
	//}},
};
```

And the ```std::variant``` implementation after refactoring looks like this:

```cpp
export module BowlingGame;

import std;

export namespace Bowling
{
    class Game
    {
        struct WaitingForFirstRollWith0Bonuses;
        struct WaitingForSecondRollWith0Bonuses;
        using State = std::variant<WaitingForFirstRollWith0Bonuses, WaitingForSecondRollWith0Bonuses>;
        struct WaitingForFirstRollWith0Bonuses
        {
            State Update(int pins, int& score) const { score += pins; return WaitingForSecondRollWith0Bonuses{}; } 
        };
        struct WaitingForSecondRollWith0Bonuses
        {
            State Update(int pins, int& score) const { score += pins; return WaitingForFirstRollWith0Bonuses {}; }
        };

        int score = 0;
        State state = WaitingForFirstRollWith0Bonuses{};
    public:
        void Roll(int pins)
        {
            if ((pins < 0) || (pins > 10))
                throw std::invalid_argument("'pins' must be between 0 and 10 inclusive");
            state = std::visit([this, pins](const auto& state) { return state.Update(pins, score); }, state);
        }
        int Score() const { return score; }
    };
}

```

So, we need two classes/structs to represent the "waiting for first/second roll with no bonus" states. They don't derive from an abstract base class, so there are no virtual methods. 
But they both must have a method with the exact same signature, ```State Update(int pins, int& score) const```. Note the ```const```, as having mutable states in a state machine would be very wrong.
So in each case, the ```Update``` method updates the current score and returns the next state, ready for the next roll. 


Looks clean to me; nothing further to refactor. Let's comment in the spares test. It still fails as before.

Write just enough code to pass that and all the other tests, and then click  [next](Step12.html).
