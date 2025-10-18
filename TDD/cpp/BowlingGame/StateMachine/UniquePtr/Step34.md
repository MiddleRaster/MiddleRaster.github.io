---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 34"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step34.html
---

Having ```AddSpare``` call ```AddRoll``` and then adjusting the bonus results in this (in my favorite tabular form):
```
    void AddFirstRoll            (int pins) { AddRoll(pins); }
    void AddSecondRollOfOpenFrame(int pins) { AddRoll(pins); ++frame; }
    void AddSpare                (int pins) { AddRoll(pins); ++frame; bonusRolls = Bonus::OneBonusRoll; }
    void AddStrike()
    {
        switch (bonusRolls)
        {
        case Bonus::NoBonusRolls        : score += 10; bonusRolls = Bonus::TwoBonusRolls;        break;
        case Bonus::OneBonusRoll        : score += 20; bonusRolls = Bonus::TwoBonusRolls;        break;
        case Bonus::TwoBonusRolls       : score += 20; bonusRolls = Bonus::TwoStrikesBonusRolls; break;
        case Bonus::TwoStrikesBonusRolls: score += 30; bonusRolls = Bonus::TwoStrikesBonusRolls; break;
        }
        ++frame;
    }
```

I'm liking how the first 3 methods look. Now, to tackle the ```AddStrike``` method.

When you've done that refactoring, click [next](Step35.html).
