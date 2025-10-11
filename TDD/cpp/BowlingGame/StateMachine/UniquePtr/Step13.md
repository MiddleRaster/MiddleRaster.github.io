---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 13"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step13.html
---

Here's the refactored version:
```
    struct State
    {
        virtual std::unique_ptr<State> Update(int /*pins*/) = 0;
        virtual ~State() {}
    };
    struct ScoreUpdater : public State
    {
        int& score;
        ScoreUpdater(int& refToScore) : score(refToScore) {}
        void AddToScore(int pins) { score += pins; }
    };

    struct WaitingForFirstRoll : public ScoreUpdater
    {
        WaitingForFirstRoll(int& refToScore) : ScoreUpdater(refToScore) {}
        std::unique_ptr<State> Update(int pins) override
        {
            AddToScore(pins);
            return std::make_unique<WaitingForSecondRoll>(score);
        }
    };
    struct WaitingForSecondRoll : public ScoreUpdater
    {
        WaitingForSecondRoll(int& refToScore) : ScoreUpdater(refToScore) {}
        std::unique_ptr<State> Update(int pins) override
        {
            AddToScore(pins);
            return std::make_unique<WaitingForFirstRoll>(score);
        }
    };
```

Ok, that's better. We now have two classes, each with one responsibility. Note: I could have made the ```ScoreUpdater``` a data-member, and passed that around. Let's keep that in mind, in case it becomes useful.

In any case, I think we're ready for a spare test. How about rolling a single spare, followed by the bonus roll. 
So, for example, if we roll 4, 6, 8 and the rest 0, the score will be 26 = 10 (for the spare) + 8 (for the spare bonus) + 8 (for the first roll of the next frame).

Let's add that test and then click [next](Step14.html).
