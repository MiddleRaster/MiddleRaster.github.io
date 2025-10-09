---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 19"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step19.html
---

Here's the precondition code, in the ```Frame``` class:
```
    bool Roll(int pins)
    {
        if (pins < 0 || pins>10)
            throw std::out_of_range("'pins' is out of range");

        if (rollOne == -1) {
            rollOne = pins;
            return false;
        } else {
            rollTwo = pins;
            if (Sum() > 10)
                throw std::out_of_range("sum of pins is out of range");
            return true;
        }
    }
```

Hmm, I like to put all the preconditions at the top. Maybe we can do that here, too.
Let's do that refactoring, and then click [Next](Step20.html)
