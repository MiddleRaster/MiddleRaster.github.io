---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 2"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step2.html
---

There are several ways of failing that firts test, but here's particularly simple one:

```
#ifndef BOWLING_H
#define BOWLING_H

namespace Bowling
{
    class Game
    {
    public:
        void Roll(int /*pins*/) {}
        int Score() const
        {
            return -1;
        }
    };
}
#endif

```

That test should now fail with ```Assert failed. Expected:<0> Actual:<-1>```. If your version doesn't, please refactor to mine.

Now, it's time to write just enough source code to pass this test; not the smart code, just get the test passing.

When it's in the green state, click [Next](Step3.html)
