---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 18"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step18.html
---

The test for when the sum of two rolls in a frame is out of range looks like this:
```
    TEST_METHOD(SumOfTwoRollsTooLargeThrowsException)
    {
        Assert::ExpectException<std::out_of_range>([this] { RollMany(2, 6); }, L"the sum of rolls in a frame must be <= 10");
    }
```

When we run it the ```Assert``` fails, with ```Assert failed. the sum of rolls in a frame must be <= 10```.

(As an aside, I want to file a complaint with management at this point. The assert should have said something
like, ```Assert failed:  expected exception not thrown```. My own drop-in replacement for Microsoft's CppUnitTest.h file does this. Or maybe they have by now; I'm still using VS 2019.)

In any case, let's get this test to pass, and click [next](Step19.html).
