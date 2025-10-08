---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 12"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step12.html
---

Here's the test:

```
    TEST_METHOD(OneStrikePlusTwoBonusRollsWorks)
    {
        game.Roll(10); // strike
        game.Roll(1);
        game.Roll(2);
        RollMany(16, 0);
        Assert::AreEqual(16, game.Score());
    }
```

When we run that test, it fails with ```Assert failed. Expected:<16> Actual:<13>```.

Let's write just enough code to pass all the tests and then click [Next](Step13.html)
