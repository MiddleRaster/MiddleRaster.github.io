---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 13"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step13.html
---

We had to remove that ```if (i%2 == 1)``` and ended up with something like this:

```
        int Score() const
        {
            int score = 0;
            for(int i=0; i<20;) // make sure i is always at the start of a frame
            {
                if (rolls[i] == 10) // strike
                {
                    score += 10 + rolls[i+1] + rolls[i+2];
                    ++i;
                } else
                if (rolls[i] + rolls[i+1] == 10) // spare
                {
                    score += 10 + rolls[i+2];
                    i += 2;
                }
                else
                {
                    score += rolls[i] + rolls[i+1];
                    i += 2;
                }
            }
            return score;
        }
```

The idea is that ```i``` always points to the beginning of a new frame. And all the tests so far pass.

Let's refactor.  
1. First, let's rename ```i``` to ```frameIndex```.  Build and test.
2. Then, extract a method, ```SumTwoRolls(int index)``` and use it throughout.  Build and test.
3. Let's also extract an ```IsStrike``` method and an ```IsSpare``` method and remove the corresponding comments. Build and test.

If you've done what I've done, it'll look like this:
```
class Game
{
    std::array<int, 21> rolls;
    int currentRoll = 0;

    int SumTwoRolls(int index) const { return rolls[index] + rolls[index + 1]; }
    bool IsStrike  (int index) const { return rolls[index] == 10; }
    bool IsSpare   (int index) const { return SumTwoRolls(index) == 10; }

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
        for(int frameIndex=0; frameIndex<20;) // frameIndex will always be at the start of a frame
        {
            if (IsStrike(frameIndex))
            {
                score += 10 + SumTwoRolls(frameIndex+1);
                ++frameIndex;
            } else
            if (IsSpare(frameIndex))
            {
                score += 10 + rolls[frameIndex+2];
                frameIndex += 2;
            }
            else
            {
                score += SumTwoRolls(frameIndex);
                frameIndex += 2;
            }
        }
        return score;
    }
};
```

Now, I see that if the spare and regular case, we've got a little duplication, that ```frameIndex += 2;```.

I wonder if I can refactor that cleanly, turning each ```if``` clause into a one-liner.

Let's give that a try, and click [Next](Step14.html)
