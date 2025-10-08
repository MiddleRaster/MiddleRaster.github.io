---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 15"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step15.html
---

The ```enum``` and the method on whose return type we want to switch look like this:
```
    enum FrameType { Strike, Spare, Open };
    FrameType FrameTypeAt(int index) const
    {
        if (IsStrike(index)) return FrameType::Strike;
        if (IsSpare (index)) return FrameType::Spare;
                             return FrameType::Open;
    }
```

And the ```Score``` method now looks like this:
```
    int Score() const
    {
        int score = 0;
        for(int frameIndex=0; frameIndex<20;) // frameIndex will always be at the start of a frame
        {
            switch (FrameTypeAt(frameIndex))
            {
            case FrameType::Strike: score += 10 + SumTwoRolls(++frameIndex);   continue;
            case FrameType::Spare:  score += 10 +         rolls[frameIndex+2]; break;
            case FrameType::Open:   score +=      SumTwoRolls(  frameIndex);   break;
            }
            frameIndex += 2;
        }
        return score;
    }
```

That's not terrible, but I'll let you decide if you think it's clearer this way or the original multiple ```if```s way - your choice.

As for me, I want to keep going and to extract a ```GetOneRoll``` method, to help line things up, in the ```FrameType::Spare:``` case.  Like this:
```
    int Score() const
    {
        int score = 0;
        for(int frameIndex=0; frameIndex<20;) // frameIndex will always be at the start of a frame
        {
            switch (FrameTypeAt(frameIndex))
            {
            case FrameType::Strike: score += 10 + SumTwoRolls(++frameIndex  ); continue;
            case FrameType::Spare:  score += 10 + GetOneRoll (  frameIndex+2); break;
            case FrameType::Open:   score +=      SumTwoRolls(  frameIndex  ); break;
            }
            frameIndex += 2;
        }
        return score;
    }
```

I can't think of anything else to refactor so let's write more tests. How about a test for all strikes, the perfect game test?

Let's write that, and click [Next](Step16.html)
