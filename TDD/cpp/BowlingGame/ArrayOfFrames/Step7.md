---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 7"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step7.html
---

We can use the handy ```Assert::ExpectException```, like so:

```
TEST_METHOD(PinsArgumentIsInRange)
{
    Assert::ExpectException<std::out_of_range>([this]{ game.Roll(-1); }, L"'pins' cannot be negative");
    Assert::ExpectException<std::out_of_range>([this]{ game.Roll(11); }, L"'pins' must be <= 10");
}
```

Two Asserts in one test:  some TDDers feel inordinately strongly that this shouldn't be done. I'm not one of those, though I usually follow that advice.

When we build and test, the first ```Assert``` fires with ```Assert failed. 'pins' cannot be negative```.

Now let's add just enough code to pass all the tests so far.

When you've done that, click [Next](Step8.html)
