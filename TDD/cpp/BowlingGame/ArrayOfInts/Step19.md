---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 19"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step19.html
---

The code needed some significant changes, though. Here's the whole class:

```
class Game
{
    std::array<int, 21> rolls;
    int currentRoll = 0;
    int validScore  = 0;

    int SumTwoRolls  (int index) const { return       rolls[index] + rolls[index+1]; }
    int GetOneRoll   (int index) const { return       rolls[index]; }
    bool IsStrike    (int index) const { return       rolls[index] == 10; }
    bool IsSpare     (int index) const { return SumTwoRolls(index) == 10; }
    int SumValidFrame(int index) const
    {
        int sum = SumTwoRolls(index);
        if (sum > 10)
            throw std::out_of_range("sum of rolls in frame too large");
        return sum;
    }

    enum FrameType { Strike, Spare, Open };
    FrameType FrameTypeAt(int index) const
    {
        if (IsStrike(index)) return FrameType::Strike;
        if (IsSpare (index)) return FrameType::Spare;
                                return FrameType::Open;
    }
    int ValidateScore() const
    {
        int score=0, rollIndex=0;
        for (int frame=0; frame<10; ++frame)
        {
            switch (FrameTypeAt(rollIndex))
            {
            case Strike: score += 10 + SumTwoRolls(++rollIndex  ); continue;
            case Spare:  score += 10 + GetOneRoll (  rollIndex+2); break;
            case Open:   score +=    SumValidFrame(  rollIndex  ); break;
            }
            rollIndex += 2;
        }
        return score;
    }

public:
    void Roll(int pins)
    {
        if (pins < 0 || pins > 10)
            throw std::out_of_range("'pins' is out of range");
        rolls[currentRoll++] = pins;

        validScore = ValidateScore();
    }
    int Score() const { return validScore; }
};    
```
I added a data-member, ```validScore```, to hold onto the score, which is now calculated in the ```Roll``` method. The reason for that is that we need to figure out all the frame boundaries,
and all that work was being done in the (former) ```Score``` method. The only case where the frame's sum might be too big is the ```FrameType::Open``` type.
That case now calls a method called ```SumValidFrame``` which sums up the rolls *and* throws an exception if the sum is too large. Since all the work to calculate the score is done now each time ```Roll``` os called,
the implementation of ```Score``` now just returns the last value of ```validScore```.

Now, have three mutable data-members violates a personal rule of mine, so I'm going to remove the data-member and call ```ValidateScore``` from the ```Score``` method.

To see the final code and tests (which is refactored a tiny bit for clarity), click [Next](Step20.html)
