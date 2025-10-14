---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 20"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step20.html
---

To make the first strike test pass, I changed the code like this:
```
    class Scorer
    {
        int bonusRolls = 0;
        int score = 0;
        int frame = 0;
    public:
        void AddFirstRoll            (int pins) { AddRoll(pins); }
        void AddSecondRollOfOpenFrame(int pins) { AddRoll(pins); ++frame; }
        void AddSpare(int pins)
        {
            score += pins;
            bonusRolls = 1;
            ++frame;
        }
        void AddStrike()
        {
            score += 10;
            bonusRolls = 2;
            ++frame;
        }
        int Score() const { return score; }
    private:
        void AddRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRolls == 0)))
                throw std::out_of_range("can't roll after game has ended");

            if (frame < 10)
                score += pins;
            if (bonusRolls == 1) {
                score += pins;
                bonusRolls = 0;
            }
            if (bonusRolls == 2) {
                score += pins;
                bonusRolls = 1;
            }
        }
    };

    struct WaitingForFirstRoll : public State
    {
        std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
        {
            if (pins == 10) {
                scorer.AddStrike();
                return std::make_unique<WaitingForFirstRoll>();
            } else {
                scorer.AddFirstRoll(pins);
                return std::make_unique<WaitingForSecondRoll>(pins);
            }
        }
    };
```

The big change is to change the ```bool bonusRoll``` to ```int bonusRolls```. And that worries me a little, as we now are hanging onto 3 ints in a row. Maybe I'll change this to an enum, later.

I also added an ```AddStrike``` method to call from the ```WaitingForFirstRoll::Update``` method.

I'm going to do an optional refactoring that I like but I'm not sure if most people would like it, and that is to make the code look more tabular. 
The ```WaitingForFirstRoll::Update``` method comes out like this:
```
    struct WaitingForFirstRoll : public State
    {
        std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
        {
            if (pins == 10) { scorer.AddStrike();        return std::make_unique<WaitingForFirstRoll >();     }
            else            { scorer.AddFirstRoll(pins); return std::make_unique<WaitingForSecondRoll>(pins); }
        }
    };
```
while the ```Scorer::AddRoll``` method comes out like this:
```
        void AddRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRolls == 0))) // Bonus::NoBonusRolls)))
                throw std::out_of_range("can't roll after game has ended");
            if (frame < 10)
                score += pins;
            if (bonusRolls == 1) { score += pins; bonusRolls = 0; }
            if (bonusRolls == 2) { score += pins; bonusRolls = 1; }
        }
```

Nothing further to refactor that I can see, so let's write the next test.  I know that two strikes in a row causes some overlapping bonus rolls, so let's write that next, say 2 strikes, then a 1 and a 2.

After you've written that test, click [next](Step21.html).
