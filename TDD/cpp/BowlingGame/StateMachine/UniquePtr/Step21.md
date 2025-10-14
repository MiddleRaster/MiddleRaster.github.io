---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 21"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step21.html
---

The two strikes in a row test looks like this:
```
    TEST_METHOD(TwoStrikesInARowPlusBonusesWork)
    {
        game.Roll(10); // strike!
        game.Roll(10); // strike!
        game.Roll(1);
        game.Roll(2);
        RollMany(14, 0);
        Assert::AreEqual(37, game.Score());
    }
```

Let's add just enough code to pass this test, too, and then click [next](Step22.html).
