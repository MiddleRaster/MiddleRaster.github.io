---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 12"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step12.html
---

The whole thing currently looks like this:
```
#ifndef BOWLING_H
#define BOWLING_H

#include <numeric>

namespace Bowling
{
    class Game
    {
        class Frame
        {
            int rollOne=-1, rollTwo=0;
        public:
            bool Roll(int pins)
            {
                if (pins < 0 || pins>10)
                    throw std::out_of_range("'pins' is out of range");

                if (rollOne == -1) {
                    rollOne = pins;
                    return false;
                } else {
                    rollTwo = pins;
                    return true;
                }
            }
            int Sum() const { return rollOne + rollTwo; }
        };
        Frame frames[10];
        int currentFrame = 0;

    public:
        void Roll(int pins)
        {
            currentFrame += frames[currentFrame].Roll(pins) ? 1 : 0;
        }
        int Score() const
        {
            return std::accumulate(std::begin(frames), std::end(frames), 0, [](int acc, const Frame& frame) { return acc + frame.Sum(); });
        }
    };
}
#endif

```

That looks clean to me; nothing further to refactor.

Let's uncomment the single spare test that we commented out early; it still fails the same way, as expected: ```Assert failed. Expected:<26> Actual:<18>```.

Now let's implement just enough code to make that test pass, too, and then click [Next](Step13.html)
