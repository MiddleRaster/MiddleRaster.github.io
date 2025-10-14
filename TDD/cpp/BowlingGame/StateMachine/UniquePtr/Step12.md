---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 12"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step12.html
---

Ok, to pass the spare test, we need to make a few changes. First, to the ```Scorer``` class:
```
    class Scorer
    {
        bool bonusRoll = false;
        int score = 0;
    public:
        void AddRoll(int pins)
        {
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
        }
        int Score() const { return score; }
    };
```

And to the two concrete ```State``` classes:
```
    struct WaitingForFirstRoll : public State
    {
        std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
        {
            scorer.AddRoll(pins);
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
            if (firstRoll + pins == 10) // spare!
                scorer.AddSpare(pins);
            else
                scorer.AddRoll(pins);
            return std::make_unique<WaitingForFirstRoll>();
        }
    };
```

We hold onto the ```firstRoll``` so that we can check if it's a spare, in the ```WaitingForSecondRoll``` class. And that class must be constructed right in the ```WaitingForFirstRoll``` class.

In addition, we added an ```AddSpare``` method to the ```Scorer``` class, which sets a ```bool``` to true to tell us to add the bonus in on whatever the next roll is.

The comment ```// spare!``` is a hint that maybe we should refactor the code to make it clearer, but I'd hate to make this class bigger just to hold an explanatory method that's only called once.
Maybe we should add that later, if it ever gets called again from somewhere else; or maybe it should go on the ```Game``` class, so that other class can call it, too. But in any case, let's wait on that.

Instead, let's write another test, an "all spares" test.

When you've written that, click [next](Step13.html).
