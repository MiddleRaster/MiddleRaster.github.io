---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 16"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step16.html
---

The fix is easy; just increase the size of the array:
```
    Frame frames[10 + 1]; // plus a secret frame
```

All the tests pass. Now the comment is hinting that this might need some refactoring, but I can't think of anything.

So instead, let's write the next test, which will be a strike, followed by 2 bonus rolls.

When that's done, click [Next](Step17.html)
