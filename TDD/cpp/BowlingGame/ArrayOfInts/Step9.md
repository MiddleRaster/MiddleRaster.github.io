---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 9"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step9.html
---

The "spare" test should look like this:

```
    TEST_METHOD(OneSparePlusBonusWorks)
    {
        game.Roll(4);
        game.Roll(6); // spare!
        game.Roll(8);
        RollMany(17, 0);
        Assert::AreEqual(26, game.Score());
    }
```

And it fails properly:  ```Assert failed. Expected:<26> Actual:<18>```.

Let's write just enough code to pass that test, and then click [Next](Step10.html)
