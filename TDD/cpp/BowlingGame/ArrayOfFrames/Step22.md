---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 22"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step22.html
---

We had to make a couple of small changes to the ```Frame``` class:
```
    class Frame
    {
        int rollOne=-1, rollTwo=0;
    public:
        bool Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");
            if ((rollOne != -1) && (rollOne+pins > 10))
                throw std::out_of_range("second 'pins' value is out of range");

            if (rollOne == -1) {
                rollOne = pins;
                if (rollOne == 10)
                    return true;
                return false;
            } else {
                rollTwo = pins;
                return true;
            }
        }
        int Score(const Frame& next) const
        {
            if (rollOne == 10)
                return 10 + next.Sum();
            if (Sum() == 10)
                return 10 + next.rollOne;
            return Sum();
        }
    private:
        int Sum() const { return rollOne + rollTwo; }
    };
```

First, in the ```Roll``` method, I return true when ```rollOne``` is 10 (a strike), so that the ```currentFrame``` counter is incremented.

Next, in the ```Score``` method, I check for ```rollOne``` being a strike, and return 10 plus the 2 bonus rolls.

Plenty of refactoring opportunities, here. So, let's refactor heavily, and then click [Next](Step23.html).
