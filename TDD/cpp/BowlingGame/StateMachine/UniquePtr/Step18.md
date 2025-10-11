---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 18"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step18.html
---

So, we introduce a ```frame``` counter, which is held in the state classes by reference, just like the ```score``` is.
The code in its entirety then looks like this:
```
class Game
{
    struct State
    {
        virtual std::unique_ptr<State> Update(int /*pins*/) = 0;
        virtual ~State() {}
    };
    struct ScoreUpdater : public State
    {
        int& score;
        ScoreUpdater(int& refToScore) : score(refToScore) {}
        void AddToScore(int pins) { score += pins; }
    };

    class WaitingForFirstRoll : public ScoreUpdater
    {
        int& frame;
        bool needBonusRoll;
    public:
        WaitingForFirstRoll(int& refToScore, int& refToFrame, bool needBonusRoll)
            : ScoreUpdater(refToScore)
            , frame       (refToFrame)
            , needBonusRoll(needBonusRoll) {}
        std::unique_ptr<State> Update(int pins) override
        {
            if (frame > 9)
                AddToScore(pins);
            else
                AddToScore(pins + (needBonusRoll ? pins : 0));
            return std::make_unique<WaitingForSecondRoll>(score, frame, pins);
        }
    };
    class WaitingForSecondRoll : public ScoreUpdater
    {
        int& frame;
        int firstRoll;
    public:
        WaitingForSecondRoll(int& refToScore, int& refToFrame, int firstRoll)
            : ScoreUpdater(refToScore)
            , frame(refToFrame)
            , firstRoll(firstRoll) {}
        std::unique_ptr<State> Update(int pins) override
        {
            AddToScore(pins);
            ++frame;
            return std::make_unique<WaitingForFirstRoll>(score, frame, firstRoll + pins == 10);
        }
    };

    int score = 0;
    int frame = 0;
    std::unique_ptr<State> state = std::make_unique<WaitingForFirstRoll>(score, frame, false);

public:
    void Roll(int pins)
    {
        if (pins < 0 || pins > 10)
            throw std::out_of_range("'pins' is out of range");

        state = state->Update(pins);
    }
    int Score() const { return score; }
};
```

The ```frame``` variable is incremented when the frame is done, which is after the second roll of a frame (either spare or open).
And it's used in the ```WaitingForFirstRoll::Update``` method to add bonus rolls only, in case we're in the last frame.

Many refactoring opportunities here:  we can move ```frame``` into the ```ScoreUpdater``` class, and maybe make it a data-member rather than using inheritance.

After making those refactorings, click [next](Step19.html).
