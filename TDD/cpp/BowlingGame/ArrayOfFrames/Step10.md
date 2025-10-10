---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 10"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step10.html
---

Adding a ```Frame``` class required some significant changes:

```
class Game
{
    class Frame
    {
        int rollOne=-1, rollTwo=0;
    public:
        bool Roll(int pins)
        {
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
        if (pins<0 || pins>10)
            throw std::out_of_range("'pins' is out of range");
            
        if (frames[currentFrame].Roll(pins) == true)
            ++currentFrame;
    }
    int Score() const
    {
        int score = 0;
        for(const auto& frame : frames)
            score += frame.Sum();
        return score;
    }
};    
```

I added the ```Frame``` class, which holds onto two pieces of data, ```rollOne``` and ```rollTwo```. To distinguish when ```pins``` refers to the first roll or second, I initialize ```rollOne``` to -1.
And there's a ```Sum``` method to return the sum of the two rolls.

Now using it, we need to know when we've had two rolls and are past a frame boundary. The ```Frame::Roll``` method returns true when the frame is complete, and we can just increment the ```currentFrame``` counter.

All the tests pass. But maybe we can refactor some more. Writing raw loops, rather than using an appropriate ```<numeric>``` algorithm, is considered poor form. 

Let's make that change and then click [Next](Step11.html)
