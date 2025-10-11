---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 3"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step3.html
---

You probably wrote exactly this:

```
class Game
{
public:
    void Roll(int /*pins*/) {}
    int Score() const
    {
        return 0;
    }
};
```

Nothing jumps out as needing refactoring to me, so let's write the next test.  How about rolling all ones? That should give a score of 20.

When you've written that second test, click [Next](Step4.html)
