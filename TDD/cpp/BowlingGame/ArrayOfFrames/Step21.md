---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 21"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step21.html
---

Here's the single strike plus bonus rolls test:
```
    TEST_METHOD(OneStrikePlusTwoBonusRollsWorks)
    {
        game.Roll(10); // strike!
        game.Roll(2);
        game.Roll(3);
        RollMany(16, 0);
        Assert::AreEqual(20, game.Score());
    }    
```

When we run that test, it throws an exception, because we fail that precondition that we just put in, in the previous step.

Let's write just enough code to pass all the tests, and then click [Next](Step22.html).
