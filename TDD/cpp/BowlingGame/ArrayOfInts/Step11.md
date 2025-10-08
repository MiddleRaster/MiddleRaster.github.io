---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 11"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step11.html
---

Here's the test:

```
    TEST_METHOD(AllSparesTest)
    {
        RollMany(21, 5);
        Assert::AreEqual(150, game.Score());
    }
```

Now, I'm expecting it just to pass, but it actually fails, with ```Assert failed. Expected:<150> Actual:<155>```.  Pretty good stuff, this TDD, huh?

The problem is that my ```for``` loop has the wrong terminating condition, and the last roll is added in twice, once as a bonus and once as the first roll of the next frame.  That second part is wrong, but it's a simple fix:
```
    for(int i=0; i<20; ++i)
```
All better. 

Still holding off on the refactoring until we have a strike test, so let's write that. 
A strike, followed by two bonus rolls (say, 1 and 2), and the rest 0 implies a score of 16 = 10 (for the strike) + 3 (for the two bonus rolls) + the next frame.

Let's write that test and then click [Next](Step12.html)
