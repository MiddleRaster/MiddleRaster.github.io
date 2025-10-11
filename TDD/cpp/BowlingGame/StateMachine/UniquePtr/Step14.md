---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 14"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step14.html
---

That first spares test should look like this:
```
        TEST_METHOD(OneSparePlusBonusWorks)
        {
            game.Roll(4);
            game.Roll(6); // spare!
            game.Roll(8);
            RollMany(17, 0);
            Assert::AreEqual(26, game.Score());
        }
```

Now, write just enough code to pass this test, and then click [next](Step15.html).
