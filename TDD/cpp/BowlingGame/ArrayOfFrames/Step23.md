---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 23"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step23.html
---

Here's the refactored ```Frame``` class:
```
    class Frame
    {
        int rollOne=-1, rollTwo=0;
    public:
        bool Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range(       "'pins' value is out of range");
            if ((rollOne != -1) && (rollOne+pins > 10))
                throw std::out_of_range("second 'pins' value is out of range");

            if (rollOne == -1) {
                rollOne = pins;
                return IsStrike();
            } else {
                rollTwo = pins;
                return true;
            }
        }
        int Score(const Frame& next) const
        {
            if (IsStrike())
                return 10 + next.Sum();
            if (IsSpare())
                return 10 + next.rollOne;
            return Sum();
        }
    private:
        bool IsStrike() const { return rollOne == 10; }
        bool IsSpare () const { return Sum()   == 10; }
        int  Sum     () const { return rollOne + rollTwo; }
    };
```

I added an ```IsStrike``` method and use it in both the ```Roll``` and ```Score``` methods.
I added an ```IsSpare``` method, for similarity and clarity.

Looks good; I'm happy with it. So, on to the next test, the perfect game test.

So, let's write that test, and then click [Next](Step24.html).
