---
layout: page
title: "Roman Numerals: Step 16"
permalink: /TDD/cpp/RomanNumerals/Step16.html
---

We're continuing to refactore the code to pass all cases up to 20. 

Here's the latest refactoring:
```
static const std::string ToRoman(int n)
{
	struct numeralData
	{
		int n;
		const char* s;
	} nd[] =
	{
		{10,  "X" },
		{ 9, "IX" },
		{ 5,  "V" },
		{ 4, "IV" },
		{ 1,  "I" },
	};

	std::string s;
	for (int i=0; i<sizeof(nd)/sizeof(numeralData); ++i)
	{
		while (n >= nd[i].n) {
			n -= nd[i].n;
			s += nd[i].s;
		}
	}
	return s;
}
```

I'm liking this.  Adding the final few elements to the data array will finish off the kata.

Uncomment all the tests and let 'er rip.

Once you're done, click [Next](Step17.html).
