---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 18"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step18.html
---

That test is easy:

```
    TEST_METHOD(FrameSumTooLargeThrowsException)
    {
        Assert::ExpectException<std::out_of_range>([this] { RollMany(2, 6); }, L"when sum of rolls in a frame too large an std::out_of_range exception is thrown");
    }
```

Go ahead and fix the problem, and when you've done that, click [Next](Step19.html)
