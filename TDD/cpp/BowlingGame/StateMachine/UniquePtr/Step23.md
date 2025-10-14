---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 23"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step23.html
---

Changing to an enum is straightforward:
```
    // only showing those methods affected by chnage to enum
    class Scorer
    {
        enum Bonus
        {
            NoBonusRolls  = 0,
            OneBonusRoll  = 1,
            TwoBonusRolls = 2,
            TwoStrikesBonusRolls = 3
        } bonusRolls = NoBonusRolls;
        int score = 0;
        int frame = 0;
    public:
        void AddSpare(int pins)
        {
            score += pins;
            bonusRolls = Bonus::OneBonusRoll;
            ++frame;
        }
        void AddStrike()
        {
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
    private:
        void AddRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRolls == 0))) // Bonus::NoBonusRolls)))
                throw std::out_of_range("can't roll after game has ended");

            if (frame < 10)
                score += pins;

            if (bonusRolls == Bonus::OneBonusRoll        ) { score += pins;   bonusRolls = Bonus::NoBonusRolls ; }
            if (bonusRolls == Bonus::TwoBonusRolls       ) { score += pins;   bonusRolls = Bonus::OneBonusRoll ; }
            if (bonusRolls == Bonus::TwoStrikesBonusRolls) { score += pins*2; bonusRolls = Bonus::TwoBonusRolls; }
        }
    };
```

Looking at the ```Addroll``` method shows that we have to do lots of handling for the bonus rolls and it's missing from the ```AddSpare``` and ```AddStrike()``` methods. That means that we're missing a few tests.

Let's fix that immediately, by adding a test for a spare followed by a strike and then a 1 and a 2, which should equal 36.

Please write that test, and then click [next](Step24.html).
