---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 29"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step29.html
---

Here's the test for a strike, then a spare and then one bonus roll, followed by all gutterballs:
```
    TEST_METHOD(StrikeThenSpareAnd1BonusRollWorks)
    {
        game.Roll(10);  // strike
        RollMany(2, 5); // spare
        game.Roll(1);
        RollMany(15, 0);
        Assert::AreEqual(32, game.Score());
    }
```

And it fails properly with ```Assert failed. Expected:<32> Actual:<27>```.

Write just enough code to pass this test, and then click [next](Step30.html).
