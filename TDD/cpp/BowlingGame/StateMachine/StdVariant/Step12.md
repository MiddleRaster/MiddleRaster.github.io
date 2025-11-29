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

That looks fine to me. But we're missing a few precondition tests:  for example, what if the user rolls a negative number of pins, or more than 10, or if the sum of the two rolls is bigger than 10.  Or if they roll more than 10 frames.
Wow, that's a lot. Let's get on it, by writing a test that checks that an exception is thrown if the user rolls a negative number of pins. Like this:

```cpp
VsTest tests[] = {
  ...
	{"a roll must be positive", []() { 
		Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(-1); });
	}},
  ...
};
```

Write just enough code to pass that and all the other tests, and then click  [next](Step13.html).
