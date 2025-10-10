---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 8"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step8.html
---

A simple fix to check the precondition we want:

```
    void Roll(int pins)
    {
        if (pins < 0 || pins > 10)
            throw std::out_of_range("'pins' is out of range");
        score += pins;
    }
```

Nothing to refactor. Let's go on to the next test.

Now, we still have a precondition that may not be met:  if two rolls in a single frame, each in range, sum up to larger than 10.  Let's save that for later, when we have implemented frames or spares or something.

For now, let's go right into rolling a single spare, followed by the bonus roll.  So, for example, if we roll 4, 6, 8 and the rest 0, the score will be 26 = 10 (for the spare) + 8 (for the spare bonus) + 8 (for the first roll of the next frame).

Let's write that test, and then click [Next](Step9.html)
