---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 26"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step26.html
---

The test for a strike and then 1 and 2 looks like this:
```
    TEST_METHOD(OneStrikePlusBonusWorks)
    {
        game.Roll(10); // strike!
        game.Roll(1);
        game.Roll(2);
        RollMany(16, 0);
        Assert::AreEqual(16, game.Score());
    }
```

And when run, it fails with an assertion having to do with that last precondition we just added. 

Go ahead and write just enough code to fix that problem, and then click [next](Step27.html).
