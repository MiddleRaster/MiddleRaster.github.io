---
layout: page_with_comments
title: "Prime Factors: Step 8"
permalink: /TDD/cpp/PrimeFactors/Step8.html
---

We're still refactoring, keeping all four test passing.

We're not using the ```std::vector<int>``` in all cases.
```
static std::vector<int> Of(int n)
{
    std::vector<int> v;
    if (n == 1)
        return v;
    if (n == 4) {
        v.push_back(2);
        n /= 2;
    }
    v.push_back(n);
    return v;
}
```

It occurs to me that we can move the first ```if``` down, and only do the last ```v.push_back(n)``` when ```n``` is not 1. That way we lose that first ```return``` statement.

Let's do that and click [Next](Step9.html).
