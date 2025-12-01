---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 35"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step35.html
---

Here are the tests:
```cpp
import tdd20;
import VsTdd20;
import std;
using namespace TDD20;

import BowlingGame;
using namespace Bowling;

void RollMany(Game& game, int rolls, int pins)
{
	for(int i=0; i<rolls; ++i)
		game.Roll(pins);
}

VsTest tests[] = {
	{"score is 0 before any rolls",          []() { Game game;                        Assert::AreEqual( 0, game.Score()); }},
	{"all gutterballs means score is 0",     []() { Game game; RollMany(game, 20, 0); Assert::AreEqual( 0, game.Score()); }},
	{"all 1 pin rolls means score is 20",    []() { Game game; RollMany(game, 20, 1); Assert::AreEqual(20, game.Score()); }},
	{"a roll must be positive",              []() { Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(-1); }); }},
	{"a roll must be <= 10",                 []() { Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(11); }); }},
	{"rolls in a frame must sum to <= 10",   []() { Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(5); game.Roll(6); }); }},
	{"cannot keep bowling past end of game", []() { Assert::ExpectingException<std::invalid_argument>([]() { Game game; RollMany(game, 21, 0); }); }},
	{"a spare plus two bonus rolls of 1 and 2 and then all gutterballs means score is 16", []() {
		Game game;
		game.Roll(5);
		game.Roll(5); // spare
		game.Roll(1);
		game.Roll(2);
		RollMany(game, 16, 0);
		Assert::AreEqual(14, game.Score());
	}},
	{"all spares means score is 150", []() {
		Game game;
		RollMany(game, 21, 5);
		Assert::AreEqual(150, game.Score());
	}},
	{"1 strike and bonus rolls of 1 and 2 means score is 16", []() {
		Game game;
		game.Roll(10); // strike!
		game.Roll(1);
		game.Roll(2);
		RollMany(game, 16, 0);
		Assert::AreEqual(16, game.Score());
	}},
	{"1 strike, 1 spare, a one and the rest gutterballs means score is 32", []() {
		Game game;
		game.Roll(10); // strike!
		game.Roll(5);
		game.Roll(5); // spare
		game.Roll(1);
		RollMany(game, 15, 0);
		Assert::AreEqual(32, game.Score());
	}},
	{"a strike followed by 2 rolls whose sum > 10 throws an exception", []() {
		Assert::ExpectingException<std::invalid_argument>([]()
		{
			Game game;
			game.Roll(10); // strike
			game.Roll(5);
			game.Roll(6); // too big!
		});
	}},
	{"a strike in the penultimate frame, followed by 2 rolls whose sum < 10", []() {
		Game game;
		RollMany(game, 18, 0);
		game.Roll(10); // strike
		game.Roll(1);
		game.Roll(2);
		Assert::AreEqual(13, game.Score()); // game over here.
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
	{"a strike in the penultimate frame, followed by a spare mean we get no more rolls", []() {
		Game game;
		RollMany(game, 18, 0);
		game.Roll(10); // strike
		game.Roll(5);
		game.Roll(5); // spare
		Assert::AreEqual(20, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
	{"perfect game means score is 300", []() {
		Game game;
		RollMany(game, 12, 10);
		Assert::AreEqual(300, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
	{"a spare followed by a strike and a two bonus rolls of 1 and 2 means score is 36", []() {
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
	{"18 gutterballs, then two strikes and a bonus roll of 1 means score is 21", []() {
		Game game;
		RollMany(game, 18, 0);
		game.Roll(10); // strike
		game.Roll(10); // strike
		game.Roll(1);
		Assert::AreEqual(21, game.Score()); // game is over here
		Assert::ExpectingException<std::invalid_argument>([&game]() { game.Roll(0); });
	}},
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
};
```

And here is the code:
```cpp
export module BowlingGame;

import std;

export namespace Bowling
{
    class Game
    {
        struct WaitingForFirstRollWith0Bonuses;
        struct WaitingForFirstRollWith1Bonus;
        struct WaitingForFirstRollWith2Bonuses;
        struct WaitingForFirstRollWith3Bonuses;
        struct WaitingForSecondRollWith0Bonuses;
        struct WaitingForSecondRollWith1Bonus;
        struct LastFrameWith1Bonus;
        struct LastFrameWith2Bonuses;
        struct LastFrameWith3Bonuses;
        struct GameOver;
        using State = std::variant< WaitingForFirstRollWith0Bonuses,
                                    WaitingForFirstRollWith1Bonus,
                                    WaitingForFirstRollWith2Bonuses,
                                    WaitingForFirstRollWith3Bonuses,
                                    WaitingForSecondRollWith0Bonuses, 
                                    WaitingForSecondRollWith1Bonus,
                                    LastFrameWith1Bonus,
                                    LastFrameWith2Bonuses,
                                    LastFrameWith3Bonuses,
                                    GameOver>;
        
        struct FirstRollUtils
        {
            template<typename T0, typename T1, typename T2> State update(int pins, int& score, int& frame, int bonusMultiplier) const
            {
                score += pins*bonusMultiplier;
                if (pins == 10) {
                    ++frame;
                    if (frame == 10)
                        return T0{};
                    return T1{};
                }
                return T2{pins};
            }
        };
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
                score += pins*bonusMultiplier;
                ++frame;
                if ((frame >= 10) && isSpare(pins)) return LastFrameWith1Bonus{};
                if  (frame >= 10)                   return GameOver{};
                if (isSpare(pins))                  return WaitingForFirstRollWith1Bonus{};
                                                    return WaitingForFirstRollWith0Bonuses{};
            }
        };
        struct WaitingForSecondRollWith0Bonuses : private SecondRollUtils
        {
            WaitingForSecondRollWith0Bonuses(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const { return update(pins, score, frame, 1); }
        };
        struct WaitingForSecondRollWith1Bonus : SecondRollUtils
        {
            WaitingForSecondRollWith1Bonus(int first) : SecondRollUtils(first) {}
            State Update(int pins, int& score, int& frame) const { return update(pins, score, frame, 2); }
        };

        struct WaitingForFirstRollWith0Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                return update<LastFrameWith2Bonuses, WaitingForFirstRollWith2Bonuses, WaitingForSecondRollWith0Bonuses>(pins, score, frame, 1);
            }
        };
        struct WaitingForFirstRollWith1Bonus : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                return update<LastFrameWith2Bonuses, WaitingForFirstRollWith2Bonuses, WaitingForSecondRollWith0Bonuses>(pins, score, frame, 2);
            }
        };
        struct WaitingForFirstRollWith2Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                return update<LastFrameWith2Bonuses, WaitingForFirstRollWith3Bonuses, WaitingForSecondRollWith1Bonus>(pins, score, frame, 2);
            }
        };
        struct WaitingForFirstRollWith3Bonuses : FirstRollUtils
        {
            State Update(int pins, int& score, int& frame) const
            {
                return update<LastFrameWith3Bonuses, WaitingForFirstRollWith3Bonuses, WaitingForSecondRollWith1Bonus>(pins, score, frame, 3);
            }
        };
        struct LastFrameWith1Bonus   { State Update(int   pins  , int&   score  , int& /*frame*/) const { score += pins;   return GameOver{};            } };
        struct LastFrameWith2Bonuses { State Update(int   pins  , int&   score  , int& /*frame*/) const { score += pins;   return LastFrameWith1Bonus{}; } };
        struct LastFrameWith3Bonuses { State Update(int   pins  , int&   score  , int& /*frame*/) const { score += pins*2; return LastFrameWith1Bonus{}; } };
        struct GameOver              { State Update(int /*pins*/, int& /*score*/, int& /*frame*/) const { throw std::invalid_argument("cannot keep bowling after game is over"); } };

        int frame = 0;
        int score = 0;
        State state = WaitingForFirstRollWith0Bonuses{};
    public:
        void Roll(int pins)
        {
            if ((pins < 0) || (pins > 10))
                throw std::invalid_argument("'pins' must be between 0 and 10 inclusive");

            state = std::visit([this, pins](const auto& state) { return state.Update(pins, score, frame); }, state);
        }
        int Score() const { return score; }
    };
}
```

Comments:  I rather like this implementation. All the state classes are just one-liners, with all the logic extracted into their base classes, with each state class clearly belonging to its group.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
