---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 11"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step11.html
---

Here's the refactored ```Score``` function, which is now a one-liner.

```
    int Score() const
    {
        return std::accumulate(std::begin(frames), std::end(frames), 0, [](int acc, const Frame& frame) { return acc + frame.Sum(); });
    }
```

It occurs to me that, if we move the precondition checks from the ```Game::Roll`` method to delegate to the ```Frame::Roll``` method, then I can make the ```Game::Roll``` method a one-liner, too.
I like that, as most everything is delegated to the ```Frame``` class.

Let's make that change and then click [Next](Step12.html)
