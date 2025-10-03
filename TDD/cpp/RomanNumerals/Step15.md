---
layout: page
title: "Roman Numerals: Step 15"
permalink: /TDD/cpp/RomanNumerals/Step15.html
---

We're continuing to refactore the code to pass all cases up to 20. 

Here's the refactored source:
```
static const std::string ToRoman(int n)
{
	struct numeralData
	{
		int n;
		const char* s;
	} nd[] =
	{
		{10   , "X" },
	};

	std::string s;

	for (int i=0; i<sizeof(nd)/sizeof(numeralData); ++i)
	{
		while (n >= nd[i].n) {
			n -= nd[i].n;
			s += nd[i].s;
		}
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

The ```while``` loop that used to handle the "X" case is now pulling the values it needs from the data array.  It works. 

Now handle all the other ```while``` loops similarly, by adding entries to the data array.

Let's do that.  Once that's done, click [Next](Step16.html).
