---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 18"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step18.html
---

Here's the test:
```
TEST_METHOD(FrameSumTooLargeThrowsException)
{
    Assert::ExpectException<std::out_of_range>([this] { RollMany(2, 6); }, L"when sum of rolls in a frame too large an std::out_of_range exception is thrown");
}
```

Let's write just enough code to pass all the test, and then click [Next](Step19.html)
