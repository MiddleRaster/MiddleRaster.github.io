---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 17"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step17.html
---

The all 5s test should look like this:
```
        TEST_METHOD(AllSparesMeansScoreis150)
        {
            RollMany(21, 5);
            Assert::AreEqual(150, game.Score());
        }
```

And when we run it, we get an assertion:  ```Assert failed. Expected:<150> Actual:<155>```.

The problem here is that we're not counting frames anywhere; we just keep going, one too far in this case.

Write just enough code to fix this up, and then click [next](Step18.html).
