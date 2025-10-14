---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 14"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step14.html
---

To count up the frames and to handle the last frames bonus roll properly, we change the code like this:
```
    class Scorer
    {
        bool bonusRoll = false;
        int score = 0;
        int frame = 0;
    public:
        void AddRoll(int pins)
        {
            if (frame < 10)
                score += pins;
            if (bonusRoll) {
                score += pins;
                bonusRoll = false;
            }
        }
        void AddSpare(int pins)
        {
            score += pins;
            bonusRoll = true;
            ++frame;
        }
        int Score() const { return score; }
    };
```

We added a data-member called ```frame``` and when the game is not in the special 10th frame, we add the pins normally, but when it is, we skip the adding of a regular roll and add the bonus roll only.

Now, having 3 data-members is making me nervous (because minimizing data-members in a functional class is one of my personal rules). However, it's not obvious how to change that yet, so I'm going to leave it for now.

On the other hand, now would be a great time to write a test that throws an exception if too many rolls are bowled.

Let's write that test, and then click [next](Step15.html).
