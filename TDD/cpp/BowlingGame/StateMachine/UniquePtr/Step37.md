---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 37"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step37.html
---

Here are all the tests:
```
#include "CppUnitTest.h"
#include "Bowling.h"

using namespace Microsoft::VisualStudio::CppUnitTestFramework;

namespace BowlingTests
{
    TEST_CLASS(BowlingGameTests)
    {
        Bowling::Game game;

        void RollMany(int rolls, int pins)
        {
            for (int i=0; i<rolls; ++i)
                game.Roll(pins);
        }
    public:
        TEST_METHOD(AllGutterBallsMeansScoreIs0)
        {
            RollMany(20, 0);
            Assert::AreEqual(0, game.Score());
        }
        TEST_METHOD(AllOnesMeanMeansScoreIs20)
        {
            RollMany(20, 1);
            Assert::AreEqual(20, game.Score());
        }
        TEST_METHOD(PinsArgumentIsInRange)
        {
            Assert::ExpectException<std::out_of_range>([this](){ game.Roll(-1); }, L"'pins' cannot be negative");
            Assert::ExpectException<std::out_of_range>([this](){ game.Roll(11); }, L"'pins' must be <= 10");
        }
        TEST_METHOD(OneSparePlusBonusWorks)
        {
            game.Roll(4);
            game.Roll(6); // spare!
            game.Roll(8);
            RollMany(17, 0);
            Assert::AreEqual(26, game.Score());
        }
        TEST_METHOD(AllSparesMeansScoreis150)
        {
            RollMany(21, 5);
            Assert::AreEqual(150, game.Score());
        }
        TEST_METHOD(BowlingAfterGameIsOverThrowsException)
        {
            Assert::ExpectException<std::logic_error>([this](){ RollMany(21, 2); }, L"Game over, man!");
        }
        TEST_METHOD(SumOfTwoRollsTooLargeThrowsException)
        {
            Assert::ExpectException<std::out_of_range>([this](){ RollMany(2, 6); }, L"the sum of rolls in a frame must be <= 10");
        }
        TEST_METHOD(OneStrikePlusBonusWorks)
        {
            game.Roll(10); // strike!
            game.Roll(1);
            game.Roll(2);
            RollMany(16, 0);
            Assert::AreEqual(16, game.Score());
        }
        TEST_METHOD(TwoStrikesInARowPlusBonusesWork)
        {
            game.Roll(10); // strike!
            game.Roll(10); // strike!
            game.Roll(1);
            game.Roll(2);
            RollMany(14, 0);
            Assert::AreEqual(37, game.Score());
        }
        TEST_METHOD(SpareStrikeAndTwoRollsWorks)
        {
            game.Roll(5);
            game.Roll(5);  // spare!
            game.Roll(10); // strike!
            game.Roll(1);
            game.Roll(2);
            RollMany(14, 0);
            Assert::AreEqual(36, game.Score());
        }
        TEST_METHOD(ThreeStrikesAndTwoRollsWorks)
        {
            RollMany(3, 10); // 3 strikes
            game.Roll(1);
            game.Roll(2);
            RollMany(12, 0);
            Assert::AreEqual(67, game.Score());
        }

        TEST_METHOD(StrikeThenSpareAnd1BonusRollWorks)
        {
            game.Roll(10);  // strike
            RollMany(2, 5); // spare
            game.Roll(1);
            RollMany(15, 0);
            Assert::AreEqual(32, game.Score());
        }

        TEST_METHOD(TwoStrikesThenSpareAnd1BonusRollWorks)
        {
            RollMany(2, 10); // 2 strikes
            RollMany(2, 5); // spare
            game.Roll(1);
            RollMany(13, 0);
            Assert::AreEqual(57, game.Score());
        }
        TEST_METHOD(ThreeStrikesThenSpareAnd1BonusRollWorks)
        {
            RollMany(3, 10); // 3 strikes
            RollMany(2, 5); // spare
            game.Roll(1);
            RollMany(11, 0);
            Assert::AreEqual(87, game.Score());
        }

        TEST_METHOD(SpareInLastFrameDrainsProperly)
        {
            Assert::ExpectException<std::logic_error>([this](){ RollMany(22, 5); }, L"Game over, man!");
        }
        TEST_METHOD(PerfectGame)
        {
            RollMany(12, 10);
            Assert::AreEqual(300, game.Score());
        }
    };
}
```

<br><br>

And here's the source:
```
#pragma once

#ifndef BOWLING_H
#define BOWLING_H

#include <memory>

namespace Bowling
{
    class Game
    {
        class Scorer
        {
            enum class Bonus { NoBonusRolls = 0, OneBonusRoll, TwoBonusRolls, TwoStrikesBonusRolls } bonusRolls = Bonus::NoBonusRolls;
            int score = 0;
            int frame = 0;
        public:
            void AddFirstRoll            (int pins) { AddRoll(pins); }
            void AddSecondRollOfOpenFrame(int pins) { AddRoll(pins); ++frame; }
            void AddSpare                (int pins) { AddRoll(pins); ++frame; bonusRolls = Bonus::OneBonusRoll; }
            void AddStrike()
            {
                AddRoll(10);
                if (++frame == 11)
                    bonusRolls = Bonus::OneBonusRoll; // the last frame's roll can only count once, even if the previous 2 frames were strikes
                else
                    bonusRolls = (bonusRolls == Bonus::NoBonusRolls 
                                              ? Bonus::TwoBonusRolls            // at least two bonus rolls for the next frame
                                              : Bonus::TwoStrikesBonusRolls);   // if two strikes in a row
            }
            int Score() const { return score; }
        private:
            void AddRoll(int pins)
            {
                if (!((frame <=  9) ||                                                          // any frames < 10 are ok
                    (((frame == 10) || (frame == 11)) && (bonusRolls != Bonus::NoBonusRolls)))) // frames 10 and 11 are ok only if there is at least one bonus roll
                    throw std::out_of_range("can't roll after game has ended");

                if (frame < 10)
                    score += pins;

                switch (bonusRolls)
                {
                case Bonus::NoBonusRolls        : score += pins*0; bonusRolls = Bonus::NoBonusRolls;  break;
                case Bonus::OneBonusRoll        : score += pins*1; bonusRolls = Bonus::NoBonusRolls;  break;
                case Bonus::TwoBonusRolls       : score += pins*1; bonusRolls = Bonus::OneBonusRoll;  break;
                case Bonus::TwoStrikesBonusRolls: score += pins*2; bonusRolls = Bonus::TwoBonusRolls; break;
                }
            }
        };

        struct State
        {
            virtual std::unique_ptr<State> Update(Scorer& /*scorer*/, int /*pins*/) const = 0;
            virtual ~State() {}
        };
        struct WaitingForFirstRoll : public State
        {
            std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
            {
                if (pins == 10) { scorer.AddStrike();        return std::make_unique<WaitingForFirstRoll >();     }
                else            { scorer.AddFirstRoll(pins); return std::make_unique<WaitingForSecondRoll>(pins); }
            }
        };
        class WaitingForSecondRoll : public State
        {
            int firstRoll;
        public:
            WaitingForSecondRoll(int firstRoll) : firstRoll(firstRoll) {}
            std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
            {
                if (firstRoll + pins > 10)
                    throw std::out_of_range("the sum of rolls in a frame must be <= 10");

                if (firstRoll + pins == 10)
                    scorer.AddSpare(pins);
                else
                    scorer.AddSecondRollOfOpenFrame(pins);
                return std::make_unique<WaitingForFirstRoll>();
            }
        };
        Scorer scorer;
        std::unique_ptr<State> state = std::make_unique<WaitingForFirstRoll>();
    public:
        void Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");
            state = state->Update(scorer, pins);
        }
        int Score() const { return scorer.Score(); }
    };
}
#endif
```

Overall, I'm somehwat disappointed. It's way more code than I'd hoped and the ```Scorer``` class could be cleaner. For alternative implementations, see the other Bowling Game kata steps, [here](/TDD/cpp/BowlingGame/BowlingGame.html).

Are we done? If you don't think so, feel free to add more tests and run them.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
