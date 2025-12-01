---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 34"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step34.html
---

We can extract the entire contents of the ```WaitingForFirstRollWithXBonuses``` classes into a single method template:
```cpp
  template<typename T0, typename T1, typename T2> State update(int pins, int& score, int& frame, int bonusMultiplier) const
  {
    score += pins*bonusMultiplier;
    if (IsStrike(pins)) {
      ++frame;
      if (frame == 10)
        return T0{};
      return T1{};
    }
    return T2{pins};
  }
```
And each of those ```WaitingForFirstRollWithXBonuses``` classes' ```Update``` methods collapses to a single line.

Click [next](Step35.html) to see the final code and tests in their entirety.
