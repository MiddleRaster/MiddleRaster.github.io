---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 25"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step25.html
---

Adding an ```if``` to the ```Scorer::AddStrike``` method that checks for having rolled a spare in the previous frame (and thus setting the bonus state to ```Bonus::OneBonusRoll```) does it:
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

            score += 10;
            bonusRolls = Bonus::TwoBonusRolls;
            ++frame;
        }
```
Now there's lots of duplication here that I want to refactor, but I'm worried about those yet missing tests more. Let's add another one first. 
We're missing the ```TwoStrikesBonusRolls``` state in the ```AddStrike``` method:  3 strikes in a row, followed by a 1 and a 2 should do it.

Please write that test, and then click [next](Step26.html).

(And after that test, we can work on the ```AddSpare``` method.)
