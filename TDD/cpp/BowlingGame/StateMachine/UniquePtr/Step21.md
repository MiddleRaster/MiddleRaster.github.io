---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 21"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step21.html
---

Here's the test for continuing to bowl after the game is over:
```
    TEST_METHOD(BowlingAfterGameIsOverThrowsException)
    {
        Assert::ExpectException<std::logic_error>([this] { RollMany(21, 2); }, L"Game over, man!");
    }
```

Let's add just enough code to pass this test, too, and then click [next](Step22.html).
