---
layout: page_with_comments
title: "Mocking the Standard Template Library: Step 3"
permalink: /TBCI/Step3.html
---

Writing the actual mock is straightforward:
```cpp
struct TestBase
{
    struct std : Monte::Empty::std
    {
        template<typename T=double>
        struct uniform_real_distribution
        {
            uniform_real_distribution(T, T) {}
            template <class Engine> double operator()(Engine&) const
            {
                return 0.5;
            }
        };
    };
};
```
Note how ```TestBase```'s ```std``` struct derives from ```Monte::Empty::std```.
All the forwarding mechanisms we just wrote in the last step are re-used here, except for the actual mock, which in this case always returns the middle point.

Here's the corresponding test:
```cpp
TEST_METHOD(AlwaysInsideBoundary)
{
    std::array<double, 2> lo, hi;
    lo.fill(-1.0);
    hi.fill(1.0);
    Monte::CarloT<2, TestBase> mc2(lo, hi); // two dimensional circle
    double area = mc2.run(1, [](const std::array<double, 2>& point) {   double r=0.0;
                                                                        for (double v : point)
                                                                            r += v*v; // distance formula (squared)
                                                                        return (r<=1.0) ? 1.0 : 0.0;
                                                                    });
    Assert::AreEqual(4.0, area);
}
```
Since the "random" point is now always exactly in the middle of the circle, the area reported wiill be the entire bounding box, or 4.0.
(You could of course easily write a second test were the point is always outside the circle (point (1,1), for an area of 0.0.)

And that's how it's done: we've mocked only one type out of the STL.

Back to [TBCI landing page](https://middleraster.github.io/TBCI/index.html)  
Back to [MiddleRaster's pages](https://middleraster.github.io/)
