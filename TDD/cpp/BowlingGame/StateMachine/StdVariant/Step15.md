---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 15"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step15.html
---

Passing the third precondition test, that the sum of rolls in a frame is <= 10, might look something like this:

```cpp
        class WaitingForSecondRollWith0Bonuses
        {
            int first;
        public:
            WaitingForSecondRollWith0Bonuses(int first) : first(first) {}
            State Update(int pins, int& score) const
            {
                if (first + pins > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins;
                if (first + pins == 10)
                    return WaitingForFirstRollWith1Bonus{};
                return WaitingForFirstRollWith0Bonuses {};
            }
        };
```

There's a small refactoring opportunity here:  ```first + pins``` is in there twice. Let's extract that into a private method, like this:

```cpp
        class WaitingForSecondRollWith0Bonuses
        {
            int first;
            int sumOfRolls(int pins) const { return first + pins; }
        public:
            WaitingForSecondRollWith0Bonuses(int first) : first(first) {}
            State Update(int pins, int& score) const
            {
                if (sumOfRolls(pins) > 10)
                    throw std::invalid_argument("sum of rolls in a frame must be <= 10");
                score += pins;
                if (sumOfRolls(pins) == 10)
                    return WaitingForFirstRollWith1Bonus{};
                return WaitingForFirstRollWith0Bonuses {};
            }
        };
```

Ok, onto the final precondition test, that we don't allow the bowler to keep bowling after the game is over.

```cpp
VsTest tests[] = {
  ...
  {"a bowler cannot keep bowling after the game is over", []() {
    Assert::ExpectingException<std::invalid_argument>([]()
      { Game game; RollMany(game, 21, 0); });
  }},
  ...
```

Write just enough code to pass that and all the other tests, and then click  [next](Step16.html).
