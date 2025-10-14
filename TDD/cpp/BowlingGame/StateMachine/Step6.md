---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 6"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step6.html
---

After extracting a method called ```RollMany```, the test code looks like this:

```
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
    TEST_METHOD(AllOnesMeansScoreIs20)
    {
        RollMany(20, 1);
        Assert::AreEqual(20, game.Score());
    }
};

```

Nothing further to refactor.  All combinations of pairs of numbers that don't add up to 10 will work. Time for another test. 

Maybe now would be a good time to validate that ```pins``` argument is not out of range.  Let's write such a test now.

When you've done that, click [Next](Step7.html)
