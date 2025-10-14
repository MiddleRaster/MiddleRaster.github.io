---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 15"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step15.html
---

A test to see if we've bowled too far looks something like this:
```
    TEST_METHOD(BowlingAfterGameIsOverThrowsException)
    {
        Assert::ExpectException<std::logic_error>([this] { RollMany(21, 2); }, L"Game over, man!");
    }
```

Now write just enough code to pass this test, and then click [next](Step16.html).
