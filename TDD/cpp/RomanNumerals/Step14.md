---
layout: page
title: "Roman Numerals: Step 14"
permalink: /TDD/cpp/RomanNumerals/Step14.html
---

We're refactoring the code to pass all cases up to 20. 

Here's the refactored source:
```
static const std::string ToRoman(int n)
{
	std::string s;

	while (n >= 10) {
		n -= 10;
		s += "X";
	}

	while (n == 9) {
		n -= 9;
		s += "IX";
	}

	while (n >= 5) {
		n -= 5;
		s += "V";
	}

	while (n == 4) {
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

All the ```if``` statements have been converted to ```while``` loops. But that gives me another idea:  convert all these ```while``` loops into a table-driven approach.

We can do this incrementally. I'll add an array just like I did for all the test data. But first, I'll make it work only on the 10/"X" case.

Let's do that.  Once that's done, click [Next](Step15.html).
