---
layout: page_with_comments
title: "Roman Numerals: Step 13"
permalink: /TDD/cpp/RomanNumerals/Step13.html
---

We're working on passing all cases up to 20. 

Here's the refactored source:
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

    while (n >= 1) {
        n -= 1;
        s += "I";
    }
    return s;
}
```

Now the ```for``` loop has been refactored to a ```while``` loop, for similarity.  

In fact, I could refactor all the ```if``` statements to ```while``` loops, and things would really look self-similar.  Let's do that.

Once those refactorings are done, click [Next](Step14.html).
