---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 27"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step27.html
---

Getting that test to pass means changing the ```bool``` for whether there is a bonus roll to an ```int``` specifying how many there are:
```
    class WaitingForFirstRoll : public State
    {
        ScoreUpdater& scorer;
        int numberOfBonusRolls;
    public:
        WaitingForFirstRoll(ScoreUpdater& scorerRef, int numberOfBonusRolls) : scorer(scorerRef), numberOfBonusRolls(numberOfBonusRolls) {}
        std::unique_ptr<State> Update(int pins) override
        {
            scorer.AddToScore(pins + (numberOfBonusRolls > 0 ? pins : 0));
            if (numberOfBonusRolls > 0)
                --numberOfBonusRolls;
            if (pins == 10) {
                scorer.FrameComplete();
                return std::make_unique<WaitingForFirstRoll>(scorer, numberOfBonusRolls + 2);
            }
            return std::make_unique<WaitingForSecondRoll>(scorer, pins, numberOfBonusRolls);
        }
    };
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
                    return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer);
                else
                    return std::make_unique<GameOver>();
            return std::make_unique<WaitingForFirstRoll>(scorer, IsSpare(pins) ? 1 : 0);
        }
    };
```

Now, that works for all the tests so far, but it doesn't look quite right to me. For instance, we're not limiting how many bonus rolls we'll accept in the ```WaitingForBonusRollsAfterLastFrame``` class.
We should add a similar ```numberOfBonusRolls``` data-member to that class, too. But we haven't got a failing test for that yet.

So let's write that test, and then click [next](Step28.html).
