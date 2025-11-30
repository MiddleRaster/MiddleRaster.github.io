---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 16"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step16.html
---

To check that we're not rolling past the end of the game means that we need to hang onto an ```int frame``` variable that counts what frame we're on.
It needs to be incremented when a frame is complete, that is, in our ```WaitingForSecondRollWith0Bonuses``` class. And we check for end in the ```WaitingForFirstRollWith0Bonuses``` state. Like so:

```cpp
    class Game
    {
        struct WaitingForFirstRollWith0Bonuses;
        class  WaitingForSecondRollWith0Bonuses;
        struct WaitingForFirstRollWith1Bonus;
        using State = std::variant< WaitingForFirstRollWith0Bonuses, 
                                    WaitingForSecondRollWith0Bonuses, 
                                    WaitingForFirstRollWith1Bonus>;
        struct WaitingForFirstRollWith0Bonuses
        {
            State Update(int pins, int& score, int& frame) const
            {
                if (frame >= 10)
                    throw std::invalid_argument("cannot keep bowling after game is over");
                score += pins;
                return WaitingForSecondRollWith0Bonuses{pins};
            } 
        };
        class WaitingForSecondRollWith0Bonuses
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
        public:
            WaitingForSecondRollWith0Bonuses(int first) : first(first) {}
            State Update(int pins, int& score, int& frame) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins;
                ++frame;
                if (sumOfRolls(pins) == 10)
                    return WaitingForFirstRollWith1Bonus{};
                return WaitingForFirstRollWith0Bonuses {};
            }
        };
        struct WaitingForFirstRollWith1Bonus
        {
            State Update(int pins, int& score, int& /*frame*/) const
            {
                score += pins*2;
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };

        int frame = 0;
        int score = 0;
        State state = WaitingForFirstRollWith0Bonuses{};
    public:
        void Roll(int pins)
        {
            if ((pins < 0) || (pins > 10))
                throw std::invalid_argument("'pins' must be between 0 and 10 inclusive");

            state = std::visit([this, pins](const auto& state) { return state.Update(pins, score, frame); }, state);
        }
        int Score() const { return score; }
    };
```

// TODO:  add new test idea here.



Write just enough code to pass that and all the other tests, and then click  [next](Step17.html).
