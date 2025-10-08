---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 20"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step20.html
---

The final tests:

```
#include "CppUnitTest.h"
#include "Bowling.h"

using namespace Microsoft::VisualStudio::CppUnitTestFramework;
using namespace Bowling;

namespace BowlingTests
{
    TEST_CLASS(BowlingGameTests)
    {
        Game game;

        void RollMany(int rolls, int pins)
        {
            for (int i=0; i<rolls; ++i)
                game.Roll(pins);
        }
        void RollSpare(int first)
        {
            game.Roll(first);
            game.Roll(10 - first);
        }
        void RollStrike()
        {
            game.Roll(10);
        }

    public:
        TEST_METHOD(AllGutterBallsMeansScoreIs0)
        {
            RollMany(20, 0);
            Assert::AreEqual(0, game.Score());
        }
        TEST_METHOD(AllOnesMeansScoreIs20)
        {
            RollMany(20, 1);
            Assert::AreEqual(20, game.Score());
        }
        TEST_METHOD(PinsArgumentIsInRange)
        {
            Assert::ExpectException<std::out_of_range>([this]{ game.Roll(-1); }, L"'pins' cannot be negative");
            Assert::ExpectException<std::out_of_range>([this]{ game.Roll(11); }, L"'pins' must be <= 10");
        }

        TEST_METHOD(OneSparePlusBonusWorks)
        {
            RollSpare(4);
            game.Roll(8);
            RollMany(17, 0);
            Assert::AreEqual(26, game.Score());
        }
        TEST_METHOD(AllSparesTest)
        {
            RollMany(21, 5);
            Assert::AreEqual(150, game.Score());
        }
        TEST_METHOD(OneStrikePlusTwoBonusRollsWorks)
        {
            RollStrike();
            game.Roll(1);
            game.Roll(2);
            RollMany(16, 0);
            Assert::AreEqual(16, game.Score());
        }
        TEST_METHOD(PerfectGame)
        {
            RollMany(12, 10);
            Assert::AreEqual(300, game.Score());
        }

        TEST_METHOD(FrameSumTooLargeThrowsException)
        {
            Assert::ExpectException<std::out_of_range>([this] { RollMany(2, 6); }, L"when sum of rolls in a frame too large an std::out_of_range exception is thrown");
        }
    };
}
```

And the final source:

```
#ifndef BOWLING_H
#define BOWLING_H

#include <stdexcept>
#include <array>

namespace Bowling
{
    class Game
    {
        std::array<int, 21> rolls;
        int currentRoll = 0;

        int SumTwoRolls  (int index) const { return       rolls[index] + rolls[index+1]; }
        int GetOneRoll   (int index) const { return       rolls[index]; }
        bool IsStrike    (int index) const { return       rolls[index] == 10; }
        bool IsSpare     (int index) const { return SumTwoRolls(index) == 10; }
        int SumValidFrame(int index) const
        {
            int sum = SumTwoRolls(index);
            if (sum > 10)
                throw std::out_of_range("sum of rolls in frame too large");
            return sum;
        }

        enum FrameType { Strike, Spare, Open };
        FrameType FrameTypeAt(int index) const
        {
            if (IsStrike(index)) return FrameType::Strike;
            if (IsSpare (index)) return FrameType::Spare;
                                 return FrameType::Open;
        }
        int ValidateScore() const
        {
            int score=0, rollIndex=0;
            for (int frame=0; frame<10; ++frame)
            {
                switch (FrameTypeAt(rollIndex))
                {
                case Strike: score += 10 + SumTwoRolls(++rollIndex  ); continue;
                case Spare:  score += 10 + GetOneRoll (  rollIndex+2); break;
                case Open:   score +=    SumValidFrame(  rollIndex  ); break;
                }
                rollIndex += 2;
            }
            return score;
        }

    public:
        void Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");
            rolls[currentRoll++] = pins;

            ValidateScore();
        }
        int Score() const { return ValidateScore(); }
    };
}
#endif
```

Overall, I'm a tiny bit disappointed. It's a bit more code than I expected and it could be clearer. For alternative implementations, see the other Bowling Game kata steps, [here](/TDD/cpp/BowlingGame/BowlingGame.html).

Are we done? If you don't think so, feel free to add more tests and run them.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
