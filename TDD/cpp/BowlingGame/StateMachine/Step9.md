---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 9"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step9.html
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

At this point, I want to introduce some kind of state machine, but we're in the red state. We can't refactor now, because we should always refactor when we're green to ensure our refactorings haven't inadvertantly broken anything.

So, let's comment out this latest test and instead think hard about what kind of states our state machine will have.

When you've decided on that, click [Next](Step10.html)
