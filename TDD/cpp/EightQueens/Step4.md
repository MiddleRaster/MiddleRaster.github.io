---
layout: page_with_comments
title: "Eight Queens: Step 4"
permalink: /TDD/cpp/EightQueens/Step4.html
---

My solution now looks like this:

```cpp
namespace Eight
{
  template <int N> class Queens
  {
    bool board[N][N] = {false};

    int Recurse(int depth)
    {
      if (depth >= N)
        return 1;

      int count = 0;
      for(int i=0; i<N; ++i)
      {
        if (!IsSpotUnderAttack(i, depth)) {
          board[i][depth] = true; // place queen
          count += Recurse(depth+1);
          board[i][depth] = false; // remove queen
        }
      }
      return count;
    }
    bool IsSpotUnderAttack(int file, int rank)
    { // there are three directions from which an attack can come:
      for(int f=file+0,r=rank-1; r>=0;             --r) if (board[f][r]) return true; // vertically
      for(int f=file-1,r=rank-1; r>=0 && f>=0; --f,--r) if (board[f][r]) return true; // diagonally to top-left
      for(int f=file+1,r=rank-1; r>=0 && f<N ; ++f,--r) if (board[f][r]) return true; // diagonally to top-right
      return false;
    }

  public:
    int CountSolutions() { return Recurse(0); }
  };  
}
```

If you wrote something similar, then you'll have seen the test pass.

Notice how the for loops in the ```IsSpotUnderAttack``` method is self-similar, tabular-looking; I like writing code this way because at least some class of errors would just leap off the page at me.

Should we wrap the place and removing queens in methods? Sure, if you want. In fact, that would obviate the need for the comment, so let's do that.

And while we're at it, let's change the loop variable from ```i``` to ```column``` to make it clear that we're iterating over column elements in the current row. 
In fact, let's rename ```depth``` to ```row```.

Go ahead and do those refactorings. Then click [Next](Step5.html).
