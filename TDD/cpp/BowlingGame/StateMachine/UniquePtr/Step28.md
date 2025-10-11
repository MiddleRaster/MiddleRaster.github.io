---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 28"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step28.html
---

Rolling 22 5s in a row should throw an exception.  Here's the test:
```
    TEST_METHOD(SpareInLastFrameDrainsProperly)
    {
        Assert::ExpectException<std::logic_error>([this] { RollMany(22, 5); }, L"Game over, man!");
    }
```

And it fails as expected.  Now write just enough code to pass this test, and then click [next](Step29.html).
