---
layout: page_with_comments
title: "Prime Factors: Step 2"
permalink: /TDD/cpp/PrimeFactors/Step2.html
---

You probably wrote something like this:

```
#ifndef _PRIMEFACTORS_H_
#define _PRIMEFACTORS_H_

#include <vector>

namespace FindPrime
{
    struct Factor
    {
        static std::vector<int> Of(int /*n*/)
        {
            throw "not implemented yet";
        }
    };
}

#endif    
```

There are other ways to fail the test:  returning a completely wrong value, throwing a std::exception or throwing an int, etc.

Your code will probably look a little different, but so long as the test now fails, it's all good.

The next task is to write just enough code to pass this first test.  Don't be smart and clever, just get it to pass. We'll add the cleverness during the refactoring step.

Once you've gotten into the green state, click [Next](Step3.html).
