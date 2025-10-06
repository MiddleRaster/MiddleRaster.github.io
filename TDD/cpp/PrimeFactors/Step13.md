---
layout: page_with_comments
title: "Prime Factors: Step 13"
permalink: /TDD/cpp/PrimeFactors/Step13.html
---

We're refactoring while making sure all test cases up to 9 pass.

We've used a ```candidate``` variable to take the values of 2 and 3. As ```candidate``` gets bigger, ```n``` gets smaller and the terminating condition is when ```n``` is 1.  Like so:

```
static std::vector<int> Of(int n)
{
    std::vector<int> v;

    int candidate = 2;
    while (n > 1) {
        while (n%candidate == 0) {
            v.push_back(candidate);
            n /= candidate;
        }
        ++candidate;
    }

    if (n != 1)
        v.push_back(n);
    return v;
}
```

Now, we're initializing a variable, checking for a terminating condition and incrementing that variable.  That's a ```for``` loop, not a ```while``` loop.

Let's change that and then click [Next](Step14.html).
