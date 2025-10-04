---
layout: page
title: "Roman Numerals: Step 11"
permalink: /TDD/cpp/RomanNumerals/Step11.html
---

We're working on passing all cases up to 19. 

The fix was indeed as easy as expected.  Here's the source:
```
static const std::string ToRoman(int n)
{
    std::string s;

    if (n >= 10) {
        n -= 10;
        s += "X";
    }

    if (n == 9) {
        n -= 9;
        s += "IX";
    }

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

The special handling for "IV" and "IX" is gone. We have a bunch of ```if``` blocks and a ```for``` loop. 
It seems a little odd that they are two categories like that, but I'll bet it'll become clear quite soon what the whole pattern is.

20 fails with ```Assert failed. Expected:<XX> Actual:<XVIIIII>```.

Write just enough code to pass the 20 case. Then click [Next](Step12.html).
