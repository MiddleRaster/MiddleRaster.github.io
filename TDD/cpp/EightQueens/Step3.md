---
layout: page_with_comments
title: "Eight Queens: Step 3"
permalink: /TDD/cpp/EightQueens/Step3.html
---

The most minimal solution probably looks something like this:

```cpp
namespace Eight
{
  template <int N> class Queens
  {
    int Recurse(int depth)
    {
      if (depth >= N)
        return 1;

      int count = 0;
      for(int i=0; i<N; ++i)
      {
        // if spot is not under attack
        // place queen
        count += Recurse(depth+1);
        // remove queen
      }
      return count;
    }
  public:
    int CountSolutions() { return Recurse(0); }
  };  
}
```

Note that we don't even have a board yet and didn't place/remove any queens, because a 1x1 board only has one possible spot.

Build-and-test shows that it does indeed pass as expected.

What next? Let's uncomment another test. 
Build and test. 
That test fails, as expected. After all, we need an actual board, to place and remove queens and to see if the queen is under attack.

That's a lot of code, but it's simple code.
The board is just a 2-dimensional array of bools.
Placing a queen sets an element of that array to true; remoing a queen sets the element to false.

Seeing if a spot is under attack is also straightforward: start at the current spot and move either straight up vertically, or diagonally to top-left and top-right, and see if any of those spots in the array are true.

Piece of cake.
Let's write it. Then click [Next](Step4.html).
