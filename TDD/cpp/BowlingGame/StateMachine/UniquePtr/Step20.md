---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 20"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step20.html
---

Ok, we removed the problematic ```if``` like this:

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
                return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer);
            return std::make_unique<WaitingForFirstRoll>(scorer, firstRoll + pins == 10);
        }
    };
    class WaitingForBonusRollsAfterLastFrame : public State
    {
        ScoreUpdater& scorer;
    public:
        WaitingForBonusRollsAfterLastFrame(ScoreUpdater& scorerRef) : scorer(scorerRef) {}
        std::unique_ptr<State> Update(int pins) override
        {
            scorer.AddToScore(pins);
            return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer);
        }
    };
```

So, we added a ```WaitingForBonusRollsAfterLastFrame``` state, whose ```Update``` method only sums in the bonus pins.

That's great, but what about the case where the last frame is not a spare? We'll keep right on going; instead, let's add a state whose ```Update``` method throws an exception.
As usual, we'll need a test first.  Let's roll 21 2s.

After you've written that test, click [next](Step21.html).
