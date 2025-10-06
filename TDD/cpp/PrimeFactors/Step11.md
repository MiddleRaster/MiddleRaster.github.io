---
layout: page_with_comments
title: "Prime Factors: Step 11"
permalink: /TDD/cpp/PrimeFactors/Step11.html
---

We're writing just enough code to make all test cases up to 8 to pass.

Changing the first ```if``` to a ```while``` does it.
```
static std::vector<int> Of(int n)
{
    std::vector<int> v;
    while (n%2 == 0) {
        v.push_back(2);
        n /= 2;
    }
    if (n != 1)
        v.push_back(n);
    return v;
}
```

That was simple. Nothing obvious to refactor. 

The next number, 9 fails with ```Assert failed. Expected:<3 3 > Actual:<9 >```.

Write just enough code to pass all the tests and click [Next](Step12.html).
