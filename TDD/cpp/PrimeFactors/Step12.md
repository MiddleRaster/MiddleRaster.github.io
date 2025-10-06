---
layout: page_with_comments
title: "Prime Factors: Step 12"
permalink: /TDD/cpp/PrimeFactors/Step12.html
---

We're writing just enough code to make all test cases up to 9 to pass.

The easiest, simplest thing that could possibly work is just duplicating the ```while``` loop. 

```
static std::vector<int> Of(int n)
{
    std::vector<int> v;

    while (n%2 == 0) {
        v.push_back(2);
        n /= 2;
    }
    while (n%3 == 0) {
        v.push_back(3);
        n /= 3;
    }

    if (n != 1)
        v.push_back(n);
    return v;
}
```

Now, let's refactor heavily. First, let's use a variable, ```candidate```, in the place of the 2 and 3, thus allowing us to combine the two ```while``` loops into one.

Let's do that and then click [Next](Step13.html).
