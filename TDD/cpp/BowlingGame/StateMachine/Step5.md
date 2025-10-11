---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 5"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step5.html
---

Here's probably the simplest code that passes both tests:

```
class Game
{
    int score = 0;
public:
    void Roll(int pins)
    {
        score += pins;
    }
    int Score() const
    {
        return score;
    }
};
```

Nothing to refactor in the source. But we can remove some duplication in the test code:  there's a loop in both that we can extract.

Do that, make sure both tests still pass, and click [Next](Step6.html)
