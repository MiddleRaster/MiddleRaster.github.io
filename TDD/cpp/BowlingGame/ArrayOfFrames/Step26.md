---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 26"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step26.html
---

Here's the final test code:
```
#include "CppUnitTest.h"
#include "Bowling.h"

using namespace Microsoft::VisualStudio::CppUnitTestFramework;

namespace BowlingTests
{
    TEST_CLASS(BowlingGameTests)
    {
        Bowling::Game game;
    public:
        TEST_METHOD(AllGutterBallsMEansScoreIs0)
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
            Assert::ExpectException<std::out_of_range>([this] { game.Roll(-1); }, L"'pins' cannot be negative");
            Assert::ExpectException<std::out_of_range>([this] { game.Roll(11); }, L"'pins' must be <= 10");
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
        TEST_METHOD(FrameSumTooLargeThrowsException)
        {
            Assert::ExpectException<std::out_of_range>([this] { RollMany(2, 6); }, L"when sum of rolls in a frame too large an std::out_of_range exception is thrown");
        }
        TEST_METHOD(OneStrikePlusTwoBonusRollsWorks)
        {
            RollStrike();
            game.Roll(2);
            game.Roll(3);
            RollMany(16, 0);
            Assert::AreEqual(20, game.Score());
        }
        TEST_METHOD(PerfectGame)
        {
            RollMany(12, 10);
            Assert::AreEqual(300, game.Score());
        }

    private:
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
    };
}
```

And here's the final source code:
```
#ifndef BOWLING_H
#define BOWLING_H

namespace Bowling
{
    class Game
    {
        class Frame
        {
            int rollOne=-1, rollTwo=0;
        public:
            bool Roll(int pins)
            {
                if (pins < 0 || pins > 10)
                    throw std::out_of_range(       "'pins' value is out of range");
                if ((rollOne != -1) && (rollOne+pins > 10))
                    throw std::out_of_range("second 'pins' value is out of range");

                if (rollOne == -1) {
                    rollOne = pins;
                    return IsStrike();
                } else {
                    rollTwo = pins;
                    return true;
                }
            }
            int Score(const Frame& next, const Frame& afterNext) const
            {
                if (IsStrike() && next.IsStrike()) return 20 + afterNext.rollOne;
                if (IsStrike())                    return 10 +      next.Sum();
                if (IsSpare ())                    return 10 +      next.rollOne;
                                                   return                Sum();
            }
        private:
            bool IsStrike() const { return rollOne == 10; }
            bool IsSpare () const { return Sum()   == 10; }
            int  Sum     () const { return rollOne + rollTwo; }
        };
        Frame frames[10 + 2]; // plus 2 double-secret frames
        int currentFrame = 0;

    public:
        void Roll(int pins)
        {
            currentFrame += frames[currentFrame].Roll(pins) ? 1 : 0;
        }
        int Score() const
        {
            int score = 0;
            for(int i=0; i<10; ++i)
                score += frames[i].Score(frames[i+1], frames[i+1]);
            return score;
        }
    };
}
#endif
```

It looks pretty clean and clear to me. There's only one explanatory comment, upon which I can't think of any way to improve.

Are we done? I think so, but if you don't feel confident, feel free to add more tests until you do.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
