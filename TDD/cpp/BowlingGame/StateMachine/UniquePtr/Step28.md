---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 28"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step28.html
---

Here are the refactored ```AddStrike``` and ```AddRoll``` methods (refactoring to tabular ```switch``` statement):
```
        void AddStrike()
        {
            switch (bonusRolls)
            {
            case Bonus::NoBonusRolls        : score += 10; bonusRolls = Bonus::TwoBonusRolls;        ++frame; break;
            case Bonus::OneBonusRoll        : score += 20; bonusRolls = Bonus::TwoBonusRolls;        ++frame; break;
            case Bonus::TwoBonusRolls       : score += 20; bonusRolls = Bonus::TwoStrikesBonusRolls; ++frame; break;
            case Bonus::TwoStrikesBonusRolls: score += 30; bonusRolls = Bonus::TwoStrikesBonusRolls; ++frame; break;
            }
        }
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

The tests all still pass.  But this code now makes clear two things: I'm missing a ton of tests/cases in the ```AddSpare``` method, and we're not checking for being in the last frame in the ```AddSpare``` and ```AndStrike``` method.

Looking very carefully at the ```AddRoll``` method, I see we can extract the ```switch``` case into its own method and call it from ```AddStrike``` (and in the near future, ```AddSpare```).

What to do first? One thing I often do when TDDing is keep a yellow pad next to my computer. When I think of tests I still need to write, I write a note to myself on the yellow pad so that I don't forget.

Since the ```switch``` statements in the ```AddRoll``` and ```AddStrike``` methods don't look identical yet, maybe we should concentrate on the ```AddSpare``` method, to help clear up what the commonality is.
To do that, let's write the next test:  if we've rolled a strike, followed by a spare, we need to handle the bonus rolls properly for the second roll of the spare.
So, a strike, a spare (say, two 5s) and one more roll, say a 1:  that adds up to 32.

Let's write that test, and then click [next](Step29.html).
