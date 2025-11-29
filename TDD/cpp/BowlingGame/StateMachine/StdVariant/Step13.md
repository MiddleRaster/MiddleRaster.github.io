---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 13"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step13.html
---

Passing our first precondition test looks something like this:

```cpp
        void Roll(int pins)
        {
            if (pins < 0)
                throw std::invalid_argument("'pins' must be positive");

            state = std::visit([this, pins](const auto& state) { return state.Update(pins, score); }, state);
        }
```
Straightforward enough. Next, let's write the next precondition test, the one that checks that a roll isn't too big:

```cpp
	{"a roll must be <= 10", []() {
		Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(11); });
	}},
```

Write just enough code to pass that and all the other tests, and then click  [next](Step14.html).
