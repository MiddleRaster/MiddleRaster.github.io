---
layout: page_with_comments
title: "Roman Numerals: Step 7"
permalink: /TDD/cpp/RomanNumerals/Step7.html
---

We're working on passing all cases up to 6. 

This ought to do it:
```
static const std::string ToRoman(int n)
{
    std::string s;

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

We handle both 5 and 6 by appending a "V" to the empty string s, subtracting 5 from n, falling through to the loop, which appends another "I" for the 6 case.

The specially handling for "IV" looks a little odd, but I can't think of a way to clean that up yet.  It'll probably become obvious eventually and I'll do it then.

Go ahead and move the ```/*``` down below the 7. Build and test.

It passes. So does 8.

But 9 fails, with this: ```Assert failed. Expected:<IX> Actual:<IV>```, which is the first time that it's failed in a particularly unreasonable way. All the other failures sort of added up to the right number, but that early return for "IV" is messing things up. Another hint that I should be on the lookout for a way to refactor this.

Write just enough code to pass the 9 case. Then click [Next](Step8.html).
