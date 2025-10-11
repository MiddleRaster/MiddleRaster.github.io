---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 23"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step23.html
---

An easy refactoring, often available to be done by your IDE ("Extract Method"):
```
    class WaitingForSecondRoll : public State
    {
        ScoreUpdater& scorer;
        int firstRoll;
        bool IsSpare(int pins) const { return firstRoll + pins == 10; }
    public:
        WaitingForSecondRoll(ScoreUpdater& scorerRef, int firstRoll)
            : scorer(scorerRef)
            , firstRoll(firstRoll)
        {}
        std::unique_ptr<State> Update(int pins) override
        {
            scorer.AddToScore(pins);
            scorer.FrameComplete();
            if (scorer.InLastFrame())
                if (IsSpare(pins))
                    return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer);
                else
                    return std::make_unique<GameOver>();
            return std::make_unique<WaitingForFirstRoll>(scorer, IsSpare(pins));
        }
    };
```

Now, it occurs to me that we're missing a precondition:  each roll could be less than 10, but their sum might be bigger than 10 which is illegal.

Let's write a test for that, and then click [next](Step24.html).
