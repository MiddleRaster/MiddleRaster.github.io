---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 17"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step17.html
---

The refactored ```Scorer``` class now looks like this:
```
    class Scorer
    {
        bool bonusRoll = false;
        int score = 0;
        int frame = 0;
    public:
        void AddFirstRoll            (int pins) { AddRoll(pins);          }
        void AddSecondRollOfOpenFrame(int pins) { AddRoll(pins); ++frame; }
        void AddSpare(int pins)
        {
            score += pins;
            bonusRoll = true;
            ++frame;
        }
        int Score() const { return score; }
    private:
        void AddRoll(int pins)
        {
            if ((frame > 10) || ((frame == 10) && (bonusRoll == false)))
                throw std::out_of_range("can't roll after game has ended");

            if (frame < 10)
                score += pins;
            if (bonusRoll) {
                score += pins;
                bonusRoll = false;
            }
        }
    };
```

Note how I lined up the first two methods:  I love tabular-looking code, because it makes many kinds of errors just leap off the page.

Now might be a good time to add that other precondition we mentioned earlier, about each roll being within range, but the sum might be bigger than 10.

Let's write that test, and then click [next](Step18.html).
