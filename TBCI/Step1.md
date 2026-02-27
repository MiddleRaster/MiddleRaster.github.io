---
layout: page_with_comments
title: "Mocking the Standard Template Library: Step 1"
permalink: /TBCI/Step1.html
---

In TBCI, we refactor the code into a template where the class derives from the template parameter.
In this case, we'd like to put a new definition of ```std::mt19937``` in the test version of the base class, while putting an alias of the real ```std::mt19937``` in the production code's base class.

But, first things first. Let's write a test, two actually:

```cpp
#include "CppUnitTest.h"
#include "MonteCarlo.h"

namespace MonteCarloTests
{
    using namespace Microsoft::VisualStudio::CppUnitTestFramework;

    TEST_CLASS(MonteCarloDemoTests)
    {
        TEST_METHOD(OriginalCode)
        {
            std::array<double,3> lo, hi;
            lo.fill(-1.0);
            hi.fill( 1.0);

            Monte::Carlo<3> mc3(lo, hi); // 3-dimensional sphere
            double estimate = mc3.run(1'000'000, [](const std::array<double,3>& point)  {   double r = 0.0;
                                                                                            for (double v : point) r += v*v; // distance formula (squared)
                                                                                            return (r <= 1.0) ? 1.0 : 0.0;
                                                                                        });
            auto error = std::fabs(estimate - 4.0*3.14159/3.0);
            Assert::IsTrue(error<.005, (std::wstring(L"should be close to 4/3*pi, but error was ") + std::to_wstring(error)  ).c_str());
        }
        TEST_METHOD(InvalidBoundingBoxThrows)
        {
            std::array<double,1> lo, hi;
            lo.fill( 1.0);
            hi.fill(-1.0);
            Assert::ExpectException<std::invalid_argument>([&]() { Monte::Carlo<1> mc(lo, hi); });
        }
    };
}
```
The first test runs the Monte Carlo integration on a sphere and then checks that the result is 4/3*pi (because the radius is 1).
The second test exercises the error path where the bounding box is backwards.


Now that we have that, the next step is to refactor:

```cpp

```





If you'd like to think about it for a bit, go ahead; when you're ready, click [Next](/TBCI/Step2.html).

Back to [TBCI landing page](https://middleraster.github.io/TBCI/index.html)  
Back to [MiddleRaster's pages](https://middleraster.github.io/)
