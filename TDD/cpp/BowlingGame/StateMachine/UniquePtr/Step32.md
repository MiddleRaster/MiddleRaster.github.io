---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 32"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step32.html
---

Adding the missing code to ```AddSpare``` results in this:
```
    void AddSpare(int pins)
    {
        switch (bonusRolls)
        {
        case Bonus::NoBonusRolls : score += pins;   bonusRolls = Bonus::OneBonusRoll; ++frame; break;
        case Bonus::OneBonusRoll : score += pins*2; bonusRolls = Bonus::OneBonusRoll; ++frame; break;
        case Bonus::TwoBonusRolls: score += pins*2; bonusRolls = Bonus::OneBonusRoll; ++frame; break;
        }
    }
```
Now, I could refactor out the ```++frame```, but I'm still uncertain if I need a case for the ```Bonus::TwoStrikesBonusRolls``` case.

So I'll write a quick test for 3 strikes in a row, a spare and a bonus, but it just passes. So we don't have to handle that last case.

Let's refactor both the ```AddSpare``` and ```AddStrike``` methods to move the ```++frame``` outside the ```switch```, setting us up to reuse the private ```AddRoll``` method.

When you've done that refactoring, click [next](Step33.html).
