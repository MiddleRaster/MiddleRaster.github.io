---
layout: page_with_comments
title: "Prime Factors: Step 15"
permalink: /TDD/cpp/PrimeFactors/Step15.html
---

We're still refactoring while keeping all test cases up to 9 passing.

The code now looks like this:

```
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
```

Are we done?  It looks to me like this code should work for any ```n```.

Just to be sure, uncomment the last test and click [Next](Step16.html).
