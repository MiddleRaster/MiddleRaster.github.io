---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 19"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step19.html
---

To fix the missing precondition, we change the ```WaitingForSecondRoll``` class's ```Update``` method, like so:
```
        std::unique_ptr<State> Update(Scorer& scorer, int pins) const override
        {
            if (firstRoll + pins > 10)
                throw std::out_of_range("the sum of rolls in a frame must be <= 10");
            if (firstRoll + pins == 10)
                scorer.AddSpare(pins);
            else
                scorer.AddSecondRollOfOpenFrame(pins);
            return std::make_unique<WaitingForFirstRoll>();
        }
```

We have some almost duplication: that ```firstRoll + pins``` is in there twice. I still want to refactor the second occurrance to ```IsSpare```, but then there wouldn't be any duplication so I'm going to wait for now.

For now, though, I think we're ready for a test with a strike plus two bonus rolls:  10 followed by, say 1 and 2.

Let's write that test, and then click [next](Step20.html).
