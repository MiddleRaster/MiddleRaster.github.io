---
layout: page_with_comments
title: "Prime Factors: Step 1"
permalink: /TDD/cpp/PrimeFactors/Step1.html
---

This is the first, shortest and easiest Kata.  The task is to write a function that takes an integer and returns a vector containing its prime factors, sorted from smallest to largest.
To make it absolutely clear what the requirements are, we can encode them in tests.  I'll do that for you here:

```
#include "CppUnitTest.h"
#include "PrimeFactors.h"
#include <string>

namespace Microsoft::VisualStudio::CppUnitTestFramework { // custom ToString specialization for Assert::AreEqual
    template<> inline std::wstring ToString<std::vector<int>>(const std::vector<int>& v)
    {
        std::wstringstream ss;
        for (const auto& e : v)
            ss << e << L" ";
        return ss.str();
    }
}

namespace PrimeFactorsKata
{
    using namespace Microsoft::VisualStudio::CppUnitTestFramework;

    TEST_CLASS(PrimeFactorsTests)
    {
        TEST_METHOD(NoFactorsOf1)         { Assert::AreEqual(std::vector<int>{       }, FindPrime::Factor::Of(1)); }
      //TEST_METHOD(FactorOf2Is2)         { Assert::AreEqual(std::vector<int>{2      }, FindPrime::Factor::Of(2)); }
      //TEST_METHOD(FactorOf3Is3)         { Assert::AreEqual(std::vector<int>{3      }, FindPrime::Factor::Of(3)); }
      //TEST_METHOD(FactorsOf4Are2And2)   { Assert::AreEqual(std::vector<int>{2, 2   }, FindPrime::Factor::Of(4)); }
      //TEST_METHOD(FactorsOf6Are2And3)   { Assert::AreEqual(std::vector<int>{2, 3   }, FindPrime::Factor::Of(6)); }
      //TEST_METHOD(FactorsOf8Are3Twos)   { Assert::AreEqual(std::vector<int>{2, 2, 2}, FindPrime::Factor::Of(8)); }
      //TEST_METHOD(FactorsOf9Are2Threes) { Assert::AreEqual(std::vector<int>{3, 3   }, FindPrime::Factor::Of(9)); }

      //TEST_METHOD(LargeComposite)
      //{
      //	Assert::AreEqual(std::vector<int>{2,2,3,3,5,5,7,11,13}, 
      //              FindPrime::Factor::Of(2*2*3*3*5*5*7*11*13));
      //}
    };
}
```

Note that we're using Visual Studio's native C++ unit testing harness (or my replacement). To aid in that, I've written a specialization of a function template
that is used in the ```Assert::AreEqual``` function.

Now, I've written all the tests for you, which is not how you typically do TDD, but it works as an exercise for beginners. The idea is that you can uncomment more and more of the tests as we go along.

Your first task is to write just enough source to **fail** this first test.  Because a test might be buggy in the sense that it passes by accident, we want to see a failing test first (red state);
then write enough code to make it pass (green state). Seeing the states go from red to green proves that the test is correct and our code is correct.

When you've written just enough code to **fail** this test, click [Next](Step2.html).
