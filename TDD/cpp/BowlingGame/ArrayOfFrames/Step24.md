---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 24"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step24.html
---

Here's the perfect game test:
```
    TEST_METHOD(PerfectGame)
    {
        RollMany(12, 10);
        Assert::AreEqual(300, game.Score());
    }
```

When we run the perfect game test, it throws an exception, because we're running off the end of the array.

Seems like a simple fix:  we need **two** extra frames at the end to hold the **second** bonus roll, in the case the first bonus roll was also a strike.
Just this should do it:
```
    Frame frames[10 + 2]; // plus two double-secret frames
```

But that fails, with ```Assert failed. Expected:<300> Actual:<200>```.  Pretty good stuff, this TDD, eh?

What's wrong? I'll let you figure that out and fix it. Then click [Next](Step25.html).
