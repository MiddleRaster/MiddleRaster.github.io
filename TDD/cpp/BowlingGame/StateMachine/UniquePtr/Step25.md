---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 25"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step25.html
---

In the ```WaitingForSecondRoll``` class, we can add the precondition to the ```Update``` method, like this:
```
        std::unique_ptr<State> Update(int pins) override
        {
            if (firstRoll + pins > 10)
                throw std::out_of_range("sum of rolls is greater than 10");

            scorer.AddToScore(pins);
            scorer.FrameComplete();
            if (scorer.InLastFrame())
                if (IsSpare(pins))
                    return std::make_unique<WaitingForBonusRollsAfterLastFrame>(scorer);
                else
                    return std::make_unique<GameOver>();
            return std::make_unique<WaitingForFirstRoll>(scorer, IsSpare(pins));
        }
```

Ok, it all looks good to me. I think we're ready for our first strike test. Let's roll a strike, followed by a 1 and a 2, which is a score of 16 = 10 (strike) + 3 (1+2 bonus) + 3 (1+2 in next frame).

Please write that test, and then click [next](Step26.html).
