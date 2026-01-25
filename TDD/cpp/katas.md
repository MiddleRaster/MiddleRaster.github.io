---
layout: page_with_comments
title: "C++ Dojo"
permalink: /TDD/cpp/katas.html
---

Welcome to the C++ Dojo. Here are a list of katas, easiest first:

1. [Prime Factors](PrimeFactors/Step1.html)
2. [Roman Numerals](RomanNumerals/Step1.html)
3. the [Bowling Game](BowlingGame/BowlingGame.html) kata, three different ways
4. the 8 Queens problem kata
5. Poker Hands kata

There are also some refactoring katas, to be added later:

1. Refactor the Bowling Game bonus logic class into a state-machine
2. The famous Gilded Rose refactoring kata
3. Clean up some truly awful MSDN sample code
4. Refactor an 8 Queens solution


Finally, some links to TDD test harnesses, which I evidently write as a hobby:
1. my header-only, completely portable, drop-in replacement for Visual Studio's native C++ unit testing framework:
[tdd4cpp](https://github.com/MiddleRaster/tdd4cpp)
2. Alternatively, if you can use C++20 modules, here's an [*extremely* small, macro-free, module-only](https://github.com/MiddleRaster/tdd20) test harness, with [VS test adapter](https://github.com/MiddleRaster/Tdd20.TestAdapter), if you really want to go that route (VS's Test Explorer is a bit slow for my taste).
