---
layout: page
title: "Roman Numerals: Step 17"
permalink: /TDD/cpp/RomanNumerals/Step17.html
---

We're now done.  Here's the final, entire source:
```
#ifndef _TO_ROMAN_H
#define _TO_ROMAN_H

#include <string>

namespace RomanNumerals
{
    struct Convert
    {
        static const std::string ToRoman(int n)
        {
            struct numeralData
            {
                int n;
                const char* s;
            } nd[] =
            {
                {1000,  "M" },
                { 900, "CM" },
                { 500,  "D" },
                { 400, "CD" },
                { 100,  "C" },
                {  90, "XC" },
                {  50,  "L" },
                {  40, "XL" },
                {  10,  "X" },
                {   9, "IX" },
                {   5,  "V" },
                {   4, "IV" },
                {   1,  "I" },
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
    };
}
#endif
```

I'm pretty happy with how it turned out.  It's tight, clear, easy to understand.

But it's not the only way.  Here's another one:
```
static const std::string ToRoman(int n)
{
    std::string M[] = { "","M","MM","MMM" };
    std::string C[] = { "","C","CC","CCC","CD","D","DC","DCC","DCCC","CM" };
    std::string X[] = { "","X","XX","XXX","XL","L","LX","LXX","LXXX","XC" };
    std::string I[] = { "","I","II","III","IV","V","VI","VII","VIII","IX" };
    return M[n/1000] + C[(n%1000)/100] + X[(n%100)/10] + I[(n%10)];
}
```
I like this one, too. Surprisingly, it runs several times slower than the ```while``` loop version, probably from copying all the ```std::string```s around.

When I get around to it, maybe I'll work up the steps of a Kata that ends up with that implementation.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
