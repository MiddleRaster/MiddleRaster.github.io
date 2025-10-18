---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 31"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step31.html
---

Refactoring to a ```switch``` statement that looks something like the ```switch``` statements in the other methods looks something like this:
```
    void AddSpare(int pins)
    {
        switch (bonusRolls)
        {
        case Bonus::NoBonusRolls: score += pins;   bonusRolls = Bonus::OneBonusRoll; ++frame; break;
        case Bonus::OneBonusRoll: score += pins*2; bonusRolls = Bonus::OneBonusRoll; ++frame; break;
        }
    }
```

Now, evidently I'm not smart enough to figure out if I'll need those addition ```case``` statements, so I'll write tests as if I do and see if they pass.

After writing this test,
```
    TEST_METHOD(TwoStrikesThenSpareAnd1BonusRollWorks)
    {
        RollMany(2, 10); // 2 strikes
        RollMany(2, 5); // spare
        game.Roll(1);
        RollMany(13, 0);
        Assert::AreEqual(57, game.Score());
    }
```
I see that it does indeed fail, with ```Assert failed. Expected:<57> Actual:<47>```, meaning we're missing a ```case``` statement.

Go ahead and write the code to pass this last test, too, and then click [next](Step32.html).
