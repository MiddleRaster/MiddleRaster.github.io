---
layout: page_with_comments
title: "Mocking the Standard Template Library: Step 1"
permalink: /TBCI/Step1.html
---

In TBCI, we refactor the code into a template where the class derives from the template parameter.

In this case, we'd like to put a new definition of ```std::uniform_real_distribution<>``` (alternatively we could mock ```std::mt19937```: either will do) in the **test** version of the base class, while putting an alias of the real ```std::uniform_real_distribution<>``` in the production code's base class.

But, first things first. Let's write a test, two actually:

```cpp
#include "CppUnitTest.h"
#include "MonteCarlo.h"

namespace MonteCarloTests
{
    using namespace Microsoft::VisualStudio::CppUnitTestFramework;

    TEST_CLASS(MonteCarloDemoTests)
    {
        TEST_METHOD(VolumeOfASphere)
        {
            std::array<double,3> lo, hi;
            lo.fill(-1.0);
            hi.fill( 1.0);

            Monte::Carlo<3> mc3(lo, hi); // 3-dimensional sphere
            double estimate = mc3.run(1'000'000, [](const std::array<double,3>& point)  {   double r = 0.0;
                                                                                            for (double v : point)
                                                                                                r += v*v; // distance formula (squared)
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


Now that we have that in place, the next step is to refactor towards TBCI:

```cpp
struct Empty {};

template <int D, typename Base>
class CarloT : private Base
{
    std::mt19937 rng;
    std::uniform_real_distribution<> U01;
    std::array<double, D> lo, hi;

public:
    CarloT(const std::array<double, D>& lo, const std::array<double, D>& hi, uint32_t seed = 12345)
         : rng(seed), U01(0.0, 1.0), lo(lo), hi(hi)
    {
        for (int d=0; d<D; ++d) {
            if (hi[d] <= lo[d]) {
                std::string msg = "Invalid bounding box: hi[" + std::to_string(d) + "] <= lo[" + std::to_string(d) + "]";
                throw std::invalid_argument(msg);
            }
        }
    }

    template<typename F>
    double run(int n, F payoff)
    {
        std::array<double, D> point{};
        double sum = 0.0;
        for (int i=0; i<n; ++i)
        {
            for (int d=0; d<D; ++d)
                point[d] = lo[d] + (hi[d] - lo[d])*U01(rng);
            sum += payoff(point);
        }
        double volumeOfBoundingBox = 1.0;
        for (int d=0; d<D; ++d)
            volumeOfBoundingBox *= (hi[d] - lo[d]);
        return volumeOfBoundingBox*(sum/n);
    }
};

template<int D> using Carlo = CarloT<D, Empty>;
```
So far we've done the standard TBCI refactorings:
1. Added a template parameter, ```Base```, which we'll use to inject our mock (in the test code) or nothing (in the production code).
2. Renamed ```Carlo``` to ```CarloT```.
3. Created ```struct Empty```, where we'll define our mock.
4. Added a ```using``` statement with ```struct Empty``` so that all our existing code will continue to compile.

So, how do we write the mock and write an alias for ```std::uniform_real_distribution<>```?

### The big idea

```cpp
struct Empty
{
    struct std
    {
        template<class D=double> using uniform_real_distribution = ::std::uniform_real_distribution<D>;
    };
};
```

We can't put a ```namespace``` inside our struct, but we *can* put a struct of the same name. This is called shadowing.


Now, to make our class pick up the new definitions, we'll need this line in our class:
```cpp
template <int D, typename Base>
class CarloT : private Base
{
    using std = typename Base::std;
```

Now, wouldn't it be great if we were done at this point?
Life is rarely that nice, and in this case, since we're telling the class that *all* the ```std``` types and functions now live in ```Empty```'s ```struct std```, it'll start looking for them there, and since it can't find them all there, it'll fail with compiler errors.

So, now we've got a bunch of clean-up to do: put enough ```using``` statements or forwarding machinery so that our code will compile.

If you'd like to give that a try yourself, go ahead; when you're ready, click [Next](/TBCI/Step2.html).
