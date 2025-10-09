---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 13"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step13.html
---

The ```Frame::Sum``` method now looks like this:
```
        int Sum(const Frame& next) const
        {
            if (rollOne + rollTwo == 10)
                return 10 + next.rollOne;
            return rollOne + rollTwo;
        }
```

And the ```Game::Score``` method looks like this:
```
    int Score() const
    {
        return std::accumulate(std::begin(frames), std::end(frames), 0, [](int acc, const Frame& frame) { return acc + frame.Sum((&frame)[1]); });
    }
```

We need to reach into the next ```Frame``` instance to grab the spare bonus roll, and I did this by passing it by reference into the Sum method. That ```frame.Sum((&frame)[1])``` implementation is hideous. 

Let's refactor and when that's done, click [Next](Step14.html)
