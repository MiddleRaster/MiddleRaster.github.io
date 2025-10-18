---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 33"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step33.html
---

Moving the ```++frame``` is a trivial refactoring and the ```Score``` ends up like this:
```
    void AddFirstRoll            (int pins) { AddRoll(pins); }
    void AddSecondRollOfOpenFrame(int pins) { AddRoll(pins); ++frame; }
    void AddSpare(int pins)
    {
        switch (bonusRolls)
        {
        case Bonus::NoBonusRolls : score += pins;   bonusRolls = Bonus::OneBonusRoll; break;
        case Bonus::OneBonusRoll : score += pins*2; bonusRolls = Bonus::OneBonusRoll; break;
        case Bonus::TwoBonusRolls: score += pins*2; bonusRolls = Bonus::OneBonusRoll; break;
        }
        ++frame;
    }
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
    int Score() const { return score; }
private:
    void AddRoll(int pins)
    {
        if ((frame > 10) || ((frame == 10) && (bonusRolls == Bonus::NoBonusRolls)))
            throw std::out_of_range("can't roll after game has ended");

        if (frame < 10)
            score += pins;

        switch (bonusRolls)
        {
        case Bonus::NoBonusRolls        :                                                     break;
        case Bonus::OneBonusRoll        : score += pins;   bonusRolls = Bonus::NoBonusRolls;  break;
        case Bonus::TwoBonusRolls       : score += pins;   bonusRolls = Bonus::OneBonusRoll;  break;
        case Bonus::TwoStrikesBonusRolls: score += pins*2; bonusRolls = Bonus::TwoBonusRolls; break;
        }
    }
```

Now, ```AddStrike```, ```AddSpare``` and ```AddRoll``` are similar, but not identical. 
The similar part is that each ```switch``` statement sums in the pins knocked down in this frame plus any bonuses rolls for previous frames, **and** it adjusts the bonus state.
In particular, ```AddRoll``` "decrements" each state back one (except for the ```Bonus::NoBonusRolls```). The ```AddSpare``` bonus state always ends up as ```Bonus::OneBonusRoll``` but the ```AddStrike``` bonus is different.

Let's do the easier case first:  the ```AddSpare``` method.  Refactor it to call the ```AddRoll``` method and then adjust the bonus state.

When you've done that refactoring, click [next](Step34.html).
