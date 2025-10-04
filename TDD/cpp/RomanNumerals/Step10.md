---
layout: page
title: "Roman Numerals: Step 10"
permalink: /TDD/cpp/RomanNumerals/Step10.html
---

We're working on passing all cases up to 14. 

Here's the source:
```
static const std::string ToRoman(int n)
{
    std::string s;

    if (n >= 10) {
        n -= 10;
        s += "X";
    }

    if (n == 9)
        return "IX";

    if (n >= 5) {
        n -= 5;
        s += "V";
    }

    if (n == 4) {
        n -= 4;
        s += "IV";
    }

    for (int i=0; i<n; ++i)
        s += "I";
    return s;
}
```

That special handling for "IV" is now gone and so is the early return. And I'll bet the "IX" case will works similarly.

I'm expecting 15 through 18 to work and they do. But 19 fails with  ```Assert failed. Expected:<XIX> Actual:<IX>```, as expected. It's an easy fix, just like last time.

Write just enough code to pass the 19 case. Then click [Next](Step11.html).
