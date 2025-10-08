---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 17"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step17.html
---

The problem with the code is that we're not counting frames and the perfect game runs past the last, 10th frame. The fix isn't too hard:

```
    int Score() const
    {
        int score = 0, rollIndex = 0;
        for(int frame=0; frame<10; ++frame)
        {
            switch (FrameTypeAt(rollIndex))
            {
            case Strike: score += 10 + SumTwoRolls(++rollIndex  ); continue;
            case Spare:  score += 10 + GetOneRoll (  rollIndex+2); break;
            case Open:   score +=      SumTwoRolls(  rollIndex  ); break;
            }
            rollIndex += 2;
        }
        return score;
    }
```

Now, all the tests pass.  Are we done? Well, we mentioned ealier that a non-strike and non-spare frame is never checking for the two rolls summing up to a number bigger than 10.

Let's wrote such a test, and when you've done that, click [Next](Step18.html)
