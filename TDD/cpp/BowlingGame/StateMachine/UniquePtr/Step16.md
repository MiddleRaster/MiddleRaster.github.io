---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 16"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step16.html
---

We had to change the ```Scorer``` class significantly. It turns out that we never incremented ```frame``` unless the roll was a spare. So the ```Scorer``` class now looks like this:
```
    class Scorer
    {
        bool bonusRoll = false;
        int score = 0;
        int frame = 0;
    public:
        void AddFirstRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRoll == false)))
                throw std::out_of_range("can't roll after game has ended");

            AddRoll(pins);
        }
        void AddSecondRollOfOpenFrame(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRoll == false)))
                throw std::out_of_range("can't roll after game has ended");

            AddRoll(pins);
            ++frame;
        }
        void AddRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRoll == false)))
                throw std::out_of_range("can't roll after game has ended");

            if (frame < 10)
                score += pins;
            if (bonusRoll) {
                score += pins;
                bonusRoll = false;
            }
        }
        void AddSpare(int pins)
        {
            score += pins;
            bonusRoll = true;
            ++frame;
        }
        int Score() const { return score; }
    };
```

We added two new methods, ```AddFirstRoll``` and ```AddSecondRollOfOpenFrame``` (an "open" frame is one that is neither a strike nor a spare), and increment ```frame``` in the latter.
We also added the precondition check to each new method.

Of course, we need to call these new methods, so we modified the concrete ```State``` implementations like this:
```
    struct WaitingForFirstRoll : public State
    {
        std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
        {
            scorer.AddFirstRoll(pins);
            return std::make_unique<WaitingForSecondRoll>(pins);
        }
    };
    class WaitingForSecondRoll : public State
    {
        int firstRoll;
    public:
        WaitingForSecondRoll(int firstRoll) : firstRoll(firstRoll) {}
        std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
        {
            if (firstRoll + pins == 10)
                scorer.AddSpare(pins);
            else
                scorer.AddSecondRollOfOpenFrame(pins);
            return std::make_unique<WaitingForFirstRoll>();
        }
    };
```

Plenty of things to refactor. First, the ```AddRoll``` method is only called from within the ```Scorer``` class, so it should be private.
And the precondition checks are unneeded in the new methods, because they call ```AddRoll``` which has the same precondtion checks.

Let's refactor, and then click [next](Step17.html).
