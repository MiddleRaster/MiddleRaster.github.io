---
layout: page
title: "Roman Numerals: Step 8"
permalink: /TDD/cpp/RomanNumerals/Step8.html
---

We're working on passing all cases up to 9. 

This ought to do it:
```
static const std::string ToRoman(int n)
{
    std::string s;

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

We merely handle "IX" specially. Now we've got two special handling cases, which I want to refactor somehow, but nothing comes to mind. Maybe it'll be come clear in the future.

Go ahead and move the ```/*``` down below the 10. Build and test.  It should fail with this: ```Assert failed. Expected:<X> Actual:<VIIIII>```. 

Write just enough code to pass the 10 case. Then click [Next](Step9.html).
