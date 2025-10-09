---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 15"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step15.html
---

Here's the all spares test:
```
TEST_METHOD(AllSparesTest)
{
    RollMany(21, 5);
    Assert::AreEqual(150, game.Score());
}
```

And, as expected, we get a crash, runtime assert or similar, depending on the OS.

Let's write just enough code to pass all the test and then click [Next](Step16.html)
