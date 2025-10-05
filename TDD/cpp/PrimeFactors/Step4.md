---
layout: page_with_comments
title: "Prime Factors: Step 4"
permalink: /TDD/cpp/PrimeFactors/Step4.html
---

We're working on passing the first two tests.

```
struct Factor
{
    static std::vector<int> Of(int n)
    {
        if (n == 1)
            return {};
        return {2};
    }
};
```

That'll do it. Your code make look slightly different: e.g., you may have checked for 2 rather than 1 or similar, or even used a ternary operator.  All good.

Nothing worth refactoring, so let's uncomment the next test. It should fail with ```Assert failed. Expected:<3 > Actual:<2 >```.

Now write just enough code to pass these first three tests, then click [Next](Step5.html).
