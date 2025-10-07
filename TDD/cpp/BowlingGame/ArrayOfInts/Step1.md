---
layout: page_with_comments
title: "Bowling Game: Array of Ints: Step 1"
permalink: /TDD/cpp/BowlingGame/ArrayOfInts/Step1.html
---

So, as always in TDD, we want to write a single test first. The simplest thing to test in bowling is to roll all "gutter balls," that is, missing every pin ever single time. At two tries per frame and 10 frames per games, that means rolling 0 all 20 times.  So, let's write that first test:

```
#include "CppUnitTest.h"
#include "Bowling.h"

using namespace Microsoft::VisualStudio::CppUnitTestFramework;
using namespace Bowling;

namespace BowlingTests
{
    TEST_CLASS(BowlingGameTests)
    {
        Game game;
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
