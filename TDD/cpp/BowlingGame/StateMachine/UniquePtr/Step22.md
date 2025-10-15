---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 22"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step22.html
---

To pass the "two strikes in a row" test, we need to modify these two methods in the ```Scorer``` class:
```
        void AddStrike()
        {
            if (bonusRolls == 2)
            {
                score += 20;
                bonusRolls = 3;
                ++frame;
                return;
            }

            score += 10;
            bonusRolls = 2;
            ++frame;
        }

        void AddRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRolls == 0)))
                throw std::out_of_range("can't roll after game has ended");

            if (frame < 10)
                score += pins;

            if (bonusRolls == 1) { score += pins;   bonusRolls = 0; }
            if (bonusRolls == 2) { score += pins;   bonusRolls = 1; }
            if (bonusRolls == 3) { score += pins*2; bonusRolls = 2; }
        }
```

We added a special case in the ```AddStrike``` method for when the previous roll was a strike, in which case we add 20 (equals the strike for this roll and again for the first bonus from the previous strike).
Now, I'm really uncomfortable with setting ```bonusRolls``` equal to 3, because it's misleading. It's not a count anymore (i.e., it's not that the next three rolls are all bonuses). Instead the next roll counts as a bonus twice, and the one after that as one more bonus.  

This tells me I should switch to an enum. So, let's do that refactoring first, and then click [next](Step23.html).
