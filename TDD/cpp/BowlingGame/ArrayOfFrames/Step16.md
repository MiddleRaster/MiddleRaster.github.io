---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 16"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step16.html
---

The fix is easy; just increase the size of the array:
```
    Frame frames[10 + 1]; // plus a secret frame
```

All the tests pass. Now the comment is hinting that this might need some refactoring, but I can't think of anything to do about that.

On the other hand, I see a little duplication in ```Frame::Score```.  Let's refactor that and then click [Next](Step17.html)
