---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 16"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step16.html
---

We can condense the ```WaitingForFirstRoll::Update``` method down to:
```
            std::unique_ptr<State> Update(int pins) override
            {
                AddToScore(pins + (needBonusRoll ? pins : 0));
                return std::make_unique<WaitingForSecondRoll>(score, pins);
            }
```
And the ```WaitingForSecondRoll::Update``` method down to:
```
            std::unique_ptr<State> Update(int pins) override
            {
                AddToScore(pins);
                return std::make_unique<WaitingForFirstRoll>(score, firstRoll + pins == 10);
            }
```

Both two-liners; not too bad.

I can't think of anything else to refactor. So, on to the next test:  how about an all spares test? Roll 21 fives in a row should add up to 150.

Go ahead and right that test, and then click [next](Step17.html).
