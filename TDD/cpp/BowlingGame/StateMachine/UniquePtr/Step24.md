---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 24"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step24.html
---

The test might look something like this:
```
    TEST_METHOD(SumOfTwoRollsTooLargeThrowsException)
    {
        Assert::ExpectException<std::out_of_range>([this] { RollMany(2, 6); }, L"the sum of rolls in a frame must be <= 10");
    }
```

Build and test:  it fails properly, with: ```Assert failed. the sum of rolls in a frame must be <= 10```.

Let's write just enough code to pass this test too, and then click [next](Step25.html).
