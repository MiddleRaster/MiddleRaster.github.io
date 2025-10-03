---
layout: page
title: "Roman Numerals: Step 2"
permalink: /TDD/cpp/RomanNumerals/Step2.html
---

Here's the source:

```
namespace RomanNumerals
{
	struct Convert
	{
		static const std::string ToRoman(int /*n*/)
		{
			return "I";
		}
	};
}
```

That was easy. Nothing to refactor. So we're ready for the next test.  Move the ```/*``` down one line to include test data for integers 1 and 2. 
Build and run; we should be in the red state, with ```Assert failed. Expected:<II> Actual:<I>```. (If you're not in this state, go back and redo your work until you are.)

Now we're ready to add just enough source to make the test for these two integers pass. When you've done that, click [Next](Step3.html).
