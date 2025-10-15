---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 26"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step26.html
---

Here's the final missing test for code we need to add to the ```AddStrike``` method:
```
    TEST_METHOD(ThreeStrikesAndTwoRollsWorks)
    {
        RollMany(3, 10); // 3 strikes
        game.Roll(1);
        game.Roll(2);
        RollMany(12, 0);
        Assert::AreEqual(66, game.Score());
    }
```

And when run, it fails with ```Assert failed. Expected:<67> Actual:<46>```. 

Go ahead and write just enough code to fix that problem, and then click [next](Step27.html).
