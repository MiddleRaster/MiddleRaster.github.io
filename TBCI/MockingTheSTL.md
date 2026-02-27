---
layout: page_with_comments
title: "Mocking the Standard Template Library"
permalink: /TBCI/MockingTheSTL.html
---

It's well-known that it's difficult to write test doubles/fakes/mocks/stubs for the STL.
But there are plenty of reasons to do so:  for example, writing unit tests where you're using std::chrono::system_clock::now(), or std::mt19937 (or other random number generator) or std::ofstream (might be slow), etc.

Ideally, we want to touch the legacy code as lightly as possible, so wrapping up stl types in our own types and replacing every instance with our types is out.
Similarly, silly macro tricks are out, as are linker tricks which are mostly not portable.

But "Test Base Class Injection" can handle it. If you haven't read about that yet, please do so now:  [TBCI](https://github.com/MiddleRaster/tbci)

## Problem Statement:  write tests for legacy Monte Carlo Integration code

Suppose you have this simple Monte Carlo class:

```cpp

#include <random>
#include <array>

namespace Monte
{
class Carlo
{
    std::mt19937 rng;
    std::uniform_real_distribution<> U01;
    std::array<double, D> lo, hi;

public:
    Carlo(const std::array<double, D>& lo, const std::array<double, D>& hi, uint32_t seed = 12345)
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
}
```








Back to [TBCI landing page](https://middleraster.github.io/TBCI/index.html)  
Back to [MiddleRaster's pages](https://middleraster.github.io/)
