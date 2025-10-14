---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 13"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step13.html
---

The new test looks like this:
```
    TEST_METHOD(AllSparesMeansScoreis150)
    {
        RollMany(21, 5);
        Assert::AreEqual(150, game.Score());
    }
```

When we run it, that test fails with ```Assert failed. Expected:<150> Actual:<155>```. The problem is that we're not counting frames anywhere, and we just keep on going, counting the last roll as both a bonus and a regular roll, when it should be a bonus only.

Let's fix that up and then click [next](Step14.html).
