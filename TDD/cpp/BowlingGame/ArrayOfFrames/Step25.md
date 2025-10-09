---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 25"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step25.html
---

The problem is that if the bonus roll is a strike, we need to reach into the next frame after that to get the next bonus. And that means passing **2** frames into the ```Frame::Score``` method.

So, the ```Game::Score``` method looks like this:
```
    int Score() const
    {
        int score = 0;
        for(int i=0; i<10; ++i)
            score += frames[i].Score(frames[i+1], frames[i+1]);
        return score;
    }
```
And the ```Frame::Score``` method looks like this:
```
    int Score(const Frame& next, const Frame& afterNext) const
    {
        if (IsStrike())
        {
            if (next.IsStrike())
                return 20 + afterNext.rollOne;
            return 10 + next.Sum();
        }
        if (IsSpare())
            return 10 + next.rollOne;
        return Sum();
    }
```

That should do it; all the tests pass. 

I want do some minor refactoring in the tests, to add ```RollStrike``` and ```RollSpare``` methods.

Then click [Next](Step26.html).
