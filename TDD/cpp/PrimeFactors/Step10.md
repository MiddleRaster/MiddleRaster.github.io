---
layout: page_with_comments
title: "Prime Factors: Step 10"
permalink: /TDD/cpp/PrimeFactors/Step10.html
---

We're writing just enough code to make all test cases up to 6 to pass.

Changing the first ```if``` statement to be more general, by checking for even-ness, not just 4, does it.
```
static std::vector<int> Of(int n)
{
    std::vector<int> v;
    if (n%2 == 0) {
        v.push_back(2);
        n /= 2;
    }
    if (n != 1)
        v.push_back(n);
    return v;
}
```

Nothing obvious to refactor. The next number, 7, will just pass. But 8 fails with ```Assert failed. Expected:<2 2 2 > Actual:<2 4 >```.

Write just enough code to pass all the tests and click [Next](Step11.html).
