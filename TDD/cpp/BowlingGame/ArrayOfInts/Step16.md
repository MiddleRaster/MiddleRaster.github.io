---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 16"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step16.html
---

The perfect game test looks like this:

```
    TEST_METHOD(PerfectGame)
    {
        RollMany(12, 10);
        Assert::AreEqual(300, game.Score());
    }
```

And when we run it, it fails with ```Assert failed. Expected:<300> Actual:<330>```.

What's the problem here?  I'll let you figure it out and fix it. When you've done that, click [Next](Step17.html)
