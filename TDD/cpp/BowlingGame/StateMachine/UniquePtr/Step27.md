---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 27"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step27.html
---

Adding the third ```if``` clause passes the last test:
```
        void AddStrike()
        {
            if (bonusRolls == Bonus::OneBonusRoll)
            {
                score += 20;
                bonusRolls = Bonus::TwoBonusRolls;
                ++frame;
                return;
            }
            if (bonusRolls == Bonus::TwoBonusRolls)
            {
                score += 20;
                bonusRolls = Bonus::TwoStrikesBonusRolls;
                ++frame;
                return;
            }
            if (bonusRolls == Bonus::TwoStrikesBonusRolls)
            {
                score += 30;
                bonusRolls = Bonus::TwoStrikesBonusRolls;
                ++frame;
                return;
            }

            score += 10;
            bonusRolls = Bonus::TwoBonusRolls;
            ++frame;
        }
```
There's so much commonality now that I can't take it anymore. Let's refactor all these ```if```s to ```switch``` statements, in both the ```AddStrike``` and ```AddRoll``` methods.

Do that refactoring, and then click [next](Step28.html).
