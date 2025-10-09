---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 20"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step20.html
---

Moving all the preconditions to the top of the ```Frame::Roll``` method results in:
```
    bool Roll(int pins)
    {
        if (pins < 0 || pins > 10)
            throw std::out_of_range("'pins' is out of range");
        if ((rollOne != -1) && (rollOne+pins > 10))
            throw std::out_of_range("second 'pins' value is out of range");

        if (rollOne == -1) {
            rollOne = pins;
            return false;
        } else {
            rollTwo = pins;
            return true;
        }
    }
```

I'll let you decide if that's better or not. You can revert, if you want, but as for me, I'll continue on to the next test, which is the strike test.

Let's write a strike test, where the bowler rolls a 10 and then 2 more rolls, say, 2 and 3, then the rest 0s, giving a score of 20 = 10 (strike) + 2+3 (bonus) + 2+3 (for the next frame).
After you've written that test, click [Next](Step21.html)
