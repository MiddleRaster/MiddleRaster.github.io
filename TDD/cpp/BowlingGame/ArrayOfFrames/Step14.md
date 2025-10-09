---
layout: page_with_comments
title: "Bowling Game: Array of Frames: Step 14"
permalink: /TDD/cpp/BowlingGame/ArrayOfFrames/Step14.html
---

I guess that ```std::accumulate``` idea didn't pan out so well. Here's the refactored ```Game::Score``` method:
```
    int Score() const
    {
        int score = 0;
        for(int i=0; i<10; ++i)
            score += frames[i].Score(frames[i+1]);
        return score;
    }
```

So, we don't need the ```#include <numeric>``` anymore.
And I also renamed the ```Frame```'s ```Sum``` method to ```Score```.

Now, that raw loop looks dangerous to me, as we might run right off the end.  

Let's write a test for that, say, an all spares test. When that's done, click [Next](Step15.html)
