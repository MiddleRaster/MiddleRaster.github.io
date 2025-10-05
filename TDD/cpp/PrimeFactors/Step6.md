---
layout: page_with_comments
title: "Prime Factors: Step 6"
permalink: /TDD/cpp/PrimeFactors/Step6.html
---

We're working on passing the first four tests, for n=1 through 4.

Remember, this is the un-refactored version.  Just get into the green state. We'll refactor shortly.

```
static std::vector<int> Of(int n)
{
    if (n == 1)
        return {};
    if (n == 4)
        return {2, 2};
    return {n};
}
```

This is probably the simplest code that passes the tests, but it needs work:  we can't keep going on like this, or we'll just be special-casing everything.

So, let's start refactoring. First, let's add a ```std::vector<int> v``` that we populate with the appropriate values. Let's do just that much first.

Then click [Next](Step7.html).
