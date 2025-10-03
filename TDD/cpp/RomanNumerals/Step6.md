---
layout: page
title: "Roman Numerals: Step 6"
permalink: /TDD/cpp/RomanNumerals/Step6.html
---

We're working on passing test cases 1 through 5. 

The simplest code that will work is this:
```
static const std::string ToRoman(int n)
{
	if (n == 5)
		return "V";

	if (n == 4)
		return "IV";

	std::string s;
	for (int i=0; i<n; ++i)
		s += "I";

	return s;
}
```

Ok, but it doesn't feel all that great. We're handling 5 specially, 4 specially and the rest with a loop. 
By being a little prescient, I know that I'll want to put the "V" in a std::string and let it fall down to the loop, appending enough "I" characters to handle 5, 6, 7, and 8.
I could refactor now, but let's be deliberate and save that for the next test case.

Go ahead and move the ```/*``` down below the 6. Build and test:  it should fail like this: ```Assert failed. Expected:<VI> Actual:<IIIIII>```. 

Write just enough code to pass 6. Then click [Next](Step7.html).
