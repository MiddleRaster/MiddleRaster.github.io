---
layout: page_with_comments
title: "Bowling Game: State Machine: Step 1"
permalink: /TDD/cpp/BowlingGame/StateMachine/Step1.html
---

So, as always in TDD, we want to write a single test first. The simplest thing to test in bowling is to roll all "gutter balls," that is, missing every pin every single time. At two tries per frame and 10 frames per games, that means rolling 0 all 20 times.  So, let's write that first test:

```
#include "CppUnitTest.h"
#include "Bowling.h"

using namespace Microsoft::VisualStudio::CppUnitTestFramework;

namespace BowlingTests
{
    TEST_CLASS(BowlingGameTests)
    {
        Bowling::Game game;
    public:
        TEST_METHOD(AllGutterBallsMeansScoreIs0)
        {
            for(int i=0;i<20; ++i)
                game.Roll(0);
            Assert::AreEqual(0, game.Score());
        }
    };
}
```

Now, we need to get into the red state, by writing just enough code to **fail** this test.

When you've done that, click [Next](Step2.html)
