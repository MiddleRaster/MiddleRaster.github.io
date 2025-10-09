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
            score += frames[i].Score(frames[i+1], frames[i+2]);
        return score;
    }
```
And the ```Frame::Score``` method looks like this:
```
    int Score(const Frame& next, const Frame& afterNext) const
    {
        if (IsStrike() && next.IsStrike()) return 20 + afterNext.rollOne;
        if (IsStrike())                    return 10 + next.Sum();
        if (IsSpare())                     return 10 + next.rollOne;
                                           return Sum();
    }
```

That should do it; all the tests pass. Now, before you say anything, I know I'm calling '''IsStrike''' twice on the current frame, which looks like it might be a perf issue, but in reality, modern optimizing compilers will elide the second and just hang onto the return from the first one.

I want do some minor refactoring in the tests, to add ```RollStrike``` and ```RollSpare``` methods.

Then click [Next](Step26.html).
