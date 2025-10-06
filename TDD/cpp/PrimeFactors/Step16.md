---
layout: page_with_comments
title: "Prime Factors: Step 16"
permalink: /TDD/cpp/PrimeFactors/Step16.html
---

And all the tests pass, so we're done. If you're not convinced that it's done, feel free to write some more test cases to prove it to yourself.

The entire final codelooks like this:

```
#ifndef _PRIMEFACTORS_H_
#define _PRIMEFACTORS_H_

#include <vector>

namespace FindPrime
{
    struct Factor
    {
        static std::vector<int> Of(int n)
        {
            std::vector<int> v;
            for (int candidate=2; n>1; ++candidate) {
                while (n%candidate == 0) {
                    v.push_back(candidate);
                    n /= candidate;
                }
            }
            return v;
        }
    };
}

#endif
```

I think the code ended up remarkably tight:  it got worse and worse, then better and better, which is typical of TDD.  The key is that it's looks clean and clear at the end.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
