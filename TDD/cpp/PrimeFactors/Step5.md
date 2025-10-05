---
layout: page_with_comments
title: "Prime Factors: Step 5"
permalink: /TDD/cpp/PrimeFactors/Step5.html
---

We're working on passing the first three tests, for n=1, 2 and 3.

```
static std::vector<int> Of(int n)
{
    if (n == 1)
        return {};
    return {n};
}
```

That ought to do it. If your code checked for n = 2 or 3 and doing something different based on that, refactor to mine, please.

When your code looks like mine, uncomment the next test, which is for the prime factors of 4. Build and test. It should fail with ```Assert failed. Expected:<2 2 > Actual:<4 >```.

Now write just enough code to pass these first four tests, then click [Next](Step6.html).
