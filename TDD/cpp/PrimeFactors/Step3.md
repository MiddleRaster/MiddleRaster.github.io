---
layout: page_with_comments
title: "Prime Factors: Step 3"
permalink: /TDD/cpp/PrimeFactors/Step3.html
---

We're working on passing the first test, which is that there are no prime factors of the number 1.

```
namespace FindPrime
{
    struct Factor
    {
        static std::vector<int> Of(int /*n*/)
        {
            return {};
        }
    };
}
```

Nothing to refactor that I can see. Go ahead an uncomment the second test. Build and test. It should fail with ```Assert failed. Expected:<2 > Actual:<>```. 
If you're not in this red state, go back and redo your work until you are.

Now write just enough code to pass these first two tests, then click [Next](Step4.html).
