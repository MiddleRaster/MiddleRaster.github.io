---
layout: page
title: "Roman Numerals: Step 9"
permalink: /TDD/cpp/RomanNumerals/Step9.html
---

We're working on passing all cases up to 10. 

This ought to do it:
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

    if (n == 4)
        return "IV";

    for (int i=0; i<n; ++i)
        s += "I";
    return s;
}
```

We add another ```if``` block that looks like similar to  the 5-8 case.

Still not quite clear on what to refactor, but I'm getting an inkling. I'll bet 40 and 90 will be handled specially, and then the whole pattern will repeat, with 400 and 900.

I'm expecting 11, 12 and 13 to work. And they do.  But 14 will fail.
Go ahead and move the ```/*``` down below the 14. Build and test.  It should fail with this: ```Assert failed. Expected:<XIV> Actual:<IV>```. 

Write just enough code to pass the 14 case. Then click [Next](Step10.html).
