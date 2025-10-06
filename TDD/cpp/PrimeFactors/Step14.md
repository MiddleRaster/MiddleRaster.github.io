---
layout: page_with_comments
title: "Prime Factors: Step 14"
permalink: /TDD/cpp/PrimeFactors/Step14.html
---

We're still refactoring while keeping all test cases up to 9 passing.

We've now gotten the code down to two nested loops.

```
static std::vector<int> Of(int n)
{
    std::vector<int> v;

    for(int candidate=2; n>1; ++candidate) {
        while (n%candidate == 0) {
            v.push_back(candidate);
            n /= candidate;
        }
    }

    if (n != 1)
        v.push_back(n);
    return v;
}
```

Notice that we're checking for ```n``` not being equal to 1 twice.  The second time is unnecessary.
Let's remove that and then click [Next](Step15.html).
