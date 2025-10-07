---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 4"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step4.html
---

Here's the second test that checks that rolling 20 ones gives a score of 20:

```
TEST_METHOD(AllOnesMeansScoreIs20)
{
    for(int i=0;i<20; ++i)
        game.Roll(1);
    Assert::AreEqual(20, game.Score());
}
```

We can fail this test in my favorite way:  by doing nothing. It fails with ```Assert failed. Expected:<20> Actual:<0>```.

Next, write just enough code to pass both of these test, and click [Next](Step5.html)
