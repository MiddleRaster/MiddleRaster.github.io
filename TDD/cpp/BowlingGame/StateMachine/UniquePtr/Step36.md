---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 36"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step36.html
---

To get the last test, the perfect game test, to pass required two changes.

The first was to allow a roll in the 12th frame:
```
    void AddRoll(int pins)
    {
        if (!((frame <=  9) ||                                                          // any frames < 10 are ok
            (((frame == 10) || (frame == 11)) && (bonusRolls != Bonus::NoBonusRolls)))) // frames 10 and 11 are ok only if there is at least one bonus roll
            throw std::out_of_range("can't roll after game has ended");

        if (frame < 10)
            score += pins;

        switch (bonusRolls)
        {
        case Bonus::NoBonusRolls        : score += pins*0; bonusRolls = Bonus::NoBonusRolls;  break;
        case Bonus::OneBonusRoll        : score += pins*1; bonusRolls = Bonus::NoBonusRolls;  break;
        case Bonus::TwoBonusRolls       : score += pins*1; bonusRolls = Bonus::OneBonusRoll;  break;
        case Bonus::TwoStrikesBonusRolls: score += pins*2; bonusRolls = Bonus::TwoBonusRolls; break;
        }
    }
```
You'll notice I added ```score += pins*0;``` for similarity and clarity. The optimizer will optimize it away, but it's clearer than without.


And the other change was to make sure that the very last roll was counted only once:
```
    void AddStrike()
    {
        AddRoll(10);
        if (++frame == 11)
            bonusRolls = Bonus::OneBonusRoll; // the last frame's roll can only count once, even if the previous 2 frames were strikes
        else
            bonusRolls = (bonusRolls == Bonus::NoBonusRolls 
                                      ? Bonus::TwoBonusRolls            // at least two bonus rolls for the next frame
                                      : Bonus::TwoStrikesBonusRolls);   // if two strikes in a row
    }
```
I added a couple of comments, as the 11th and 12th frames are special (read, tricky). 

And that's it. All done.

Click [next](Step37.html) to see all the code and tests in one place.
