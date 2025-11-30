---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 12"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step12.html
---

To pass the first Spare test, we need to add a new state, ```WaitingForFirstRollWith1Bonus```, and we need to detect in ```WaitingForSecondRollWith0Bonuses``` that we got a spare; something like this:

```cpp
        class WaitingForSecondRollWith0Bonuses
        {
            int first;
        public:
            WaitingForSecondRollWith0Bonuses(int first) : first(first) {}
            State Update(int pins, int& score) const
            {
                score += pins;
                if (first + pins == 10)
                    return WaitingForFirstRollWith1Bonus{};
                return WaitingForFirstRollWith0Bonuses {};
            }
        };
        struct WaitingForFirstRollWith1Bonus
        {
            State Update(int pins, int& score) const
            {
                score += pins*2;
                return WaitingForSecondRollWith0Bonuses{pins};
            }
        };
```
So we have to hang onto the first roll's value so that we can add in the current roll and see if it's a spare. If so, put us into the ```WaitingForFirstRollWith1Bonus``` state, else back to ```WaitingForFirstRollWith0Bonuses```.

That looks fine to me. But we're missing a few more precondition tests:  if the sum of the two rolls is bigger than 10; or if they roll more than 10 frames.

Let's write the next precondition test, the one that checks that the sum of two rolls in a frame isn't over 10:

```cpp
{"a frame must be <= 10", []() {
  Assert::ExpectingException<std::invalid_argument>([]()
    { Game game; game.Roll(5); game.Roll(6); });
}},
```

Write just enough code to pass that and all the other tests, and then click  [next](Step15.html).
