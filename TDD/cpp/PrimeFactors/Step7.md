---
layout: page_with_comments
title: "Prime Factors: Step 7"
permalink: /TDD/cpp/PrimeFactors/Step7.html
---

We're refactoring, keeping all four test passing.

Here's the first step of the refactoring, just adding a ```std::vector<int>```.
```
static std::vector<int> Of(int n)
{
    std::vector<int> v;
    if (n == 1)
        return v;
    if (n == 4)
        return {2, 2};
    v.push_back(n);
    return v;
}
```

We're using the vector in the case of all number other than 4.  Let's refactor some more and use it for 4, too.

Let's add that and click [Next](Step8.html).
