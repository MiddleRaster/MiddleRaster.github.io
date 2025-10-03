---
layout: page
title: "Roman Numerals: Step 5"
permalink: /TDD/cpp/RomanNumerals/Step5.html
---

To pass this test, we need to handle the IV case specially.  We could either that do that before the ```for``` loop or after. 
If we do it after, we'd be doing some work for nothing and throwing it away, which doesn't feel right. So we'll do the special handling first.

```
	struct Convert
	{
		static const std::string ToRoman(int n)
		{
			if (n == 4)
				return "IV";

			std::string s;
			for (int i=0; i<n; ++i)
				s += "I";
			return s;
		}
	};
```

Nothing to refactor that I can see.

Go ahead and move the ```/*``` down below the 5. Build and test:  it should fail like this: ```Assert failed. Expected:<V> Actual:<IIIII>```. 

Write just enough code to pass these 5 test cases. Then click [Next](Step6.html).
