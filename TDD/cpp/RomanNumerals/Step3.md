---
layout: page
title: "Roman Numerals: Step 3"
permalink: /TDD/cpp/RomanNumerals/Step3.html
---

Here's the current source, just enough to pass the first two test cases:

```
namespace RomanNumerals
{
	struct Convert
	{
		static const std::string ToRoman(int n)
		{
			if (n == 1)
				return "I";
			return "II";
		}
	};
}
```
Your source may look slightly different, e.g., maybe you checked for ```if (n > 1)``` or you had the sense the other way around.  All good.
Now let's refactor. We could either do nothing or we could be just a little prescient and realize that we can get the 3 case to pass if we used a ```for``` loop.  Let's not jump the gun and save that for the next test.

Go ahead and move the ```/*``` down below the 3 and get these 3 test cases to pass.  Then click [Next](Step4.html).
