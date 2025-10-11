---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 22"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step22.html
---

Adding a state for when the bowler is continuing to bowl after the game is over looks like this:
```
    class WaitingForSecondRoll : public State
    {
        ScoreUpdater& scorer;
        int firstRoll;
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
                if (firstRoll + pins == 10)
                    return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer);
                else
                    return std::make_unique<GameOver>();
            return std::make_unique<WaitingForFirstRoll>(scorer, firstRoll + pins == 10);
        }
    };
    class GameOver : public State
    {
        std::unique_ptr<State> Update(int /*pins*/) override
        {
            throw std::logic_error("attempting to roll after game is over.");
        }
    };
```

And all the tests pass. However, I spy a little duplication:  ```firstRoll + pins == 10``` is used twice. Let's extract a method for that.

When you've done that, click [next](Step23.html).
