---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 14"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step14.html
---

Hmm, it came out like this:
```
    int Score() const
    {
        int score = 0;
        for(int frameIndex=0; frameIndex<20;) // frameIndex will always be at the start of a frame
        {
            if (IsStrike(frameIndex))
            {
                score += 10 + SumTwoRolls(++frameIndex);
                continue;
            }
            if (IsSpare(frameIndex))    score += 10 + rolls[frameIndex+2];
            else                        score += SumTwoRolls(frameIndex);
            frameIndex += 2;
        }
        return score;
    }
```

I don't like it. But before I revert it, that ```continue``` gives me an idea:  what if we change this into a ```switch``` statement, switching on an ```enum```?

Let's give that a try, and click [Next](Step15.html)
