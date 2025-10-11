---
layout: page_with_comments
title: "Bowling Game: State Machine: Unique_Ptr: Step 19"
permalink: /TDD/cpp/BowlingGame/StateMachine/UniquePtr/Step19.html
---

Moving the ```frame``` data-member inside the ```ScoreUpdater``` class results in something like this:
```
    class ScoreUpdater
    {
        int score = 0;
        int frame = 0;
    public:
        void AddToScore(int pins) { score += pins; }
        void FrameComplete()      { ++frame; }
        bool InLastFrame() const  { return frame > 9; }
        int  GetScore   () const  { return score; }
    };
```

We use the ```ScoreUpdater``` as a data-member in the ```Game``` class:
```
    class Game
    {   /* nested classes removed for clarityy */
        ScoreUpdater scorer;
        std::unique_ptr<State> state = std::make_unique<WaitingForFirstRoll>(scorer, false);
    public:
        void Roll(int pins)
        {
            if (pins < 0 || pins > 10)
                throw std::out_of_range("'pins' is out of range");

            state = state->Update(pins);
        }
        int Score() const { return scorer.GetScore(); }
    };
```

And finally, we hold on to a ```ScoreUpdater``` reference as a data-member in the ```WaitingForFirstRoll``` and ```WaitingForSecondRoll``` class, rather than as a base, like so:
```
    class WaitingForFirstRoll : public State
    {
        ScoreUpdater& scorer;
        bool needBonusRoll;
    public:
        WaitingForFirstRoll(ScoreUpdater& scorerRef, bool needBonusRoll)
            : scorer(scorerRef)
            , needBonusRoll(needBonusRoll) {}
        std::unique_ptr<State> Update(int pins) override
        {
            if (scorer.InLastFrame())
                scorer.AddToScore(pins);
            else
                scorer.AddToScore(pins + (needBonusRoll ? pins : 0));
            return std::make_unique<WaitingForSecondRoll>(scorer, pins);
        }
    };
    class WaitingForSecondRoll : public State
    {
        ScoreUpdater& scorer;
        int firstRoll;
    public:
        WaitingForSecondRoll(ScoreUpdater& scorerRef, int firstRoll)
            : scorer(scorerRef)
            , firstRoll(firstRoll)
        {}
        std::unique_ptr<State> Update(int pins) override
        {
            scorer.AddToScore(pins);
            scorer.FrameComplete();
            return std::make_unique<WaitingForFirstRoll>(scorer, firstRoll + pins == 10);
        }
    };
```

The tests all pass.  However, there's a bit of a problem. 
In his book, "Practical Statecharts in C/C++", Miro Samek the comment that seeing an ```if``` in the ```Update``` method of a state machine might be an indication of a problem, something he calls using 'guard clauses.'
Rather than using an ```if```, we really should define another state, for when we're done with the last frame and just collecting bonus rolls.
To be clear, it's ok to use an ```if``` to decide which state to create next, but it's not ok to use an ```if``` in the ```Update``` method to decide to update differently, like we're doing, by updating the scorer with  only the bonus or with the roll plus the bonus.

Let's refactor to create another state class, and then click [next](Step20.html).
