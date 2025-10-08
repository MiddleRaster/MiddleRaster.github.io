---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 10"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step10.html
---

We've got to change the code significantly to make this work. Note that all our efforts so far are not in vain, because we have good tests that we need to keep passing forever.

A simple way to make the spare test pass is this:
```
#include <stdexcept>
#include <array>

namespace Bowling
{
    class Game
    {
        std::array<int, 21> rolls;
        int currentRoll = 0;
    public:
        void Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");
            rolls[currentRoll++] = pins;
        }
        int Score() const
        {
            int score = 0;
            for(int i=0; i<21; ++i)
            {
                score += rolls[i];
                if (i%2 == 1)                        // it's a second roll
                    if (rolls[i-1] + rolls[i] == 10) // it's a spare!
                        score += rolls[i+1];         // add the bonus
            }
            return score;
        }
    };
}
#endif

```

So, we hang onto all the rolls in an ```std::array``` (of the maximum size, where we have a spare in the last frame), populated in the ```Roll``` method, but do all the scoring in the ```Score``` method.
It works, but could use some refactoring.

Now that line, ```if (i%2 == 1)``` will be wrong if we have a strike, but we don't have a test for that yet. Even though the code is a little ugly, I'm going to hold off refactoring for a bit.

Let's write some more spare tests, like nothing but spares.  If you rolls 21 fives in a row, they're all spares and the score will be 150.

Let's write that test and then click [Next](Step11.html)
