---
layout: page
title: "Roman Numerals: Step 12"
permalink: /TDD/cpp/RomanNumerals/Step12.html
---

We're working on passing all cases up to 20. 

Here's the source:
```
static const std::string ToRoman(int n)
{
    std::string s;

    while (n >= 10) {
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

Merely changing the ```if``` to a ```while``` handles up to 20. 

Now to refactoring:  the repeated "X" is similar to the repeated "I". Let's refactor those so that they look identical. 

Once that refactoring's done, click [Next](Step13.html).
