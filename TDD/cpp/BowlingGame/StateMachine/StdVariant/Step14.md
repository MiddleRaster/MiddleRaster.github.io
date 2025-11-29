---
layout: page_with_comments
title: "Bowling Game: State Machine: Std::Variant: Step 14"
permalink: /TDD/cpp/BowlingGame/StateMachine/StdVariant/Step14.html
---

Passing the two precondition tests might look something like this:

```cpp
        void Roll(int pins)
        {
            if ((pins < 0) || (pins > 10))
                throw std::invalid_argument("'pins' must be between 0 and 10 inclusive");

            state = std::visit([this, pins](const auto& state) { return state.Update(pins, score); }, state);
        }

```
Also straightforward. 

Next, let's write the third precondition test, the one that checks that the sum of two rolls in a frame aren't over 10:

```cpp
	{"a frame must be <= 10", []() {
		Assert::ExpectingException<std::invalid_argument>([]() { Game game; game.Roll(5); game.Roll(6); });
	}},
```

Write just enough code to pass that and all the other tests, and then click  [next](Step15.html).
