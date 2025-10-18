---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 30"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step30.html
---

The simplest code to pass that last strike then spare test looks like this:
```
        void AddSpare(int pins)
        {
            if (bonusRolls == Bonus::OneBonusRoll)
            {
                score += pins*2;
                bonusRolls = Bonus::OneBonusRoll;
                ++frame;
                return;
            }

            score += pins;
            bonusRolls = Bonus::OneBonusRoll;
            ++frame;
        }
```

It's refactorable into a switch statement, but do we need those other case statements? Let's think about that as we refactor.

When you've done that, click [next](Step31.html).
