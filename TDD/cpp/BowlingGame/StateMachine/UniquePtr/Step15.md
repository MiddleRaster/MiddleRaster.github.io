---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 15"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step15.html
---

To handle the bonus incurred by rolling a spare means we need code something like this:
```
        class WaitingForFirstRoll : public ScoreUpdater
        {
            bool needBonusRoll;
        public:
            WaitingForFirstRoll(int& refToScore, bool needBonusRoll)
                : ScoreUpdater(refToScore)
                , needBonusRoll(needBonusRoll)
            {}
            std::unique_ptr<State> Update(int pins) override
            {
                AddToScore(pins);
                if (needBonusRoll == true) {
                    AddToScore(pins);
                    needBonusRoll = false;
                }
                return std::make_unique<WaitingForSecondRoll>(score, pins);
            }
        };
        class WaitingForSecondRoll : public ScoreUpdater
        {
            int firstRoll;
        public:
            WaitingForSecondRoll(int& refToScore, int firstRoll)
                : ScoreUpdater(refToScore)
                , firstRoll(firstRoll)
            {}
            std::unique_ptr<State> Update(int pins) override
            {
                AddToScore(pins);

                if (firstRoll + pins == 10)
                    return std::make_unique<WaitingForFirstRoll>(score, true);

                return std::make_unique<WaitingForFirstRoll>(score, false);
            }
        };
```
We added a flag to ```WaitingForFirstRoll``` such that if true, the ```pins``` value is added in twice, once for the roll and once for the bonus roll because it was a spare in the previous frame.

Plenty of refactoring opportunities; let's clean this up, and then click [next](Step16.html).
