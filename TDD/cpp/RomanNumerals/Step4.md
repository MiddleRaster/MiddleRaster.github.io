---
layout: page_with_comments
title: "Roman Numerals: Step 4"
permalink: /TDD/cpp/RomanNumerals/Step4.html
---

Here's the current source, just enough to pass the first 3 test cases:

```
namespace RomanNumerals
{
    struct Convert
    {
        static const std::string ToRoman(int n)
        {
            std::string s;
            for (int i=0; i<n; ++i)
                s += "I";
            return s;
        }
    };
}
```

Nothing to refactor that I can see.

Go ahead and move the ```/*``` down below the 4. Build and test; it should fail with ```Assert failed. Expected:<IV> Actual:<IIII>```.

Write just enough source to  get all 4 test cases to pass. 

Then click [Next](Step5.html).
