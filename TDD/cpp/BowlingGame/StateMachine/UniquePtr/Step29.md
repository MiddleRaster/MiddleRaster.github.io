---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 29"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step29.html
---

In the ```WaitingForSecondRoll``` class, when we're in the last frame, we have to pass an ```int``` to the ```WaitingForBonusRollsAfterLastFrame``` class:
```
    class WaitingForSecondRoll : public State
    {
        ScoreUpdater& scorer;
        int firstRoll;
        int numberOfBonusRolls;

        bool IsSpare(int pins) const { return firstRoll + pins == 10; }
    public:
        WaitingForSecondRoll(ScoreUpdater& scorerRef, int firstRoll, int numberOfBonusRolls) : scorer(scorerRef), firstRoll(firstRoll), numberOfBonusRolls(numberOfBonusRolls) {}
        std::unique_ptr<State> Update(int pins) override
        {
            if (firstRoll + pins > 10)
                throw std::out_of_range("sum of rolls is greater than 10");

            scorer.AddToScore(pins + (numberOfBonusRolls > 0 ? pins : 0));
            scorer.FrameComplete();
            if (scorer.InLastFrame())
                if (IsSpare(pins))
                    return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer, 1);
                else
                    return std::make_unique<GameOver>();
            return std::make_unique<WaitingForFirstRoll>(scorer, IsSpare(pins) ? 1 : 0);
        }
    };
    class WaitingForBonusRollsAfterLastFrame : public State
    {
        ScoreUpdater& scorer;
        int numberOfBonusRolls;
    public:
        WaitingForBonusRollsAfterLastFrame(ScoreUpdater& scorerRef, int numberOfBonusRolls) : scorer(scorerRef), numberOfBonusRolls(numberOfBonusRolls) {}
        std::unique_ptr<State> Update(int pins) override
        {
            scorer.AddToScore(pins);
            if (numberOfBonusRolls > 0)
                numberOfBonusRolls--;
            if (numberOfBonusRolls > 0)
                return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer, numberOfBonusRolls);
            else
                return std::make_unique<GameOver>();
        }
    };
```

This is getting messy. But I want to write the final perfect game test. 
Let's write that test, and then click [next](Step30.html).
