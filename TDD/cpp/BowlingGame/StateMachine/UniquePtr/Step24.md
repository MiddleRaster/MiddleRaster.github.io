---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 24"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step24.html
---

The "spare, then strike, then bonuses" test looks like this:
```
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
```

Build and test:  it fails properly, with: ```Assert failed. Expected:<36> Actual:<26>""".

Go ahead and write just enough code to pass this test too, and then click [next](Step25.html).
