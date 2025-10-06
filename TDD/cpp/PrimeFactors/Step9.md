---
layout: page_with_comments
title: "Prime Factors: Step 9"
permalink: /TDD/cpp/PrimeFactors/Step9.html
---

We're still refactoring, keeping all four test passing.

Moving that (formerly) first ```if``` down has tightened up the code a little bit. 

Now, we could check for even-ness in the (now) first ```if``` and refactor that, but we really should do that when we have a test for 6.

```
static std::vector<int> Of(int n)
{
    std::vector<int> v;
    if (n == 4) {
        v.push_back(2);
        n /= 2;
    }
    if (n != 1)
        v.push_back(n);
    return v;
}
```

The test for 5 (which isn't written) should just pass. You can write it if you like and see it pass.

In any case, uncomment the next test, the one for 6, and build and test. It should fail with: ```Assert failed. Expected:<2 3 > Actual:<6 >```.

Write just enough code to pass all the tests and click [Next](Step10.html).
