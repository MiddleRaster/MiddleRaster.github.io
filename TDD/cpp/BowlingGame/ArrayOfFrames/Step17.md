---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 17"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step17.html
---

After refactoring the ```Frame``` class, we have:
```
    int Score(const Frame& next) const
    {
        if (Sum() == 10)
            return 10 + next.rollOne;
        return Sum();
    }
private:
    int Sum() const { return rollOne + rollTwo; }
    
```

All the tests pass. It occurs to me that we're missing a precondition test:  it's possible that a single frame's two rolls each are less than 10 but the sum is greater than 10, which is illegal.

Let's write a test for that and then click [Next](Step18.html)
