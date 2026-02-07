---
layout: page_with_comments
title: "Eight Queens: Step 5"
permalink: /TDD/cpp/EightQueens/Step5.html
---

After a little refactoring, the code looks like this:

```cpp
namespace Eight
{
  template <int N> class Queens
  {
    bool board[N][N] = {false};

    int Recurse(int row)
    {
      if (row >= N)
        return 1;

      int count = 0;
      for(int column=0; column<N; ++column)
      {
        if (!IsSpotUnderAttack(column, row)) {
          PlaceQueenOn (column,row);
          count += Recurse(row+1);
          RemoveQueenAt(column, row);
        }
      }
      return count;
    }
    bool IsSpotUnderAttack(int file, int rank) const
    { // there are three directions from which an attack can come:
      for(int f=file+0,r=rank-1; r>=0;             --r) if (board[f][r]) return true; // vertically
      for(int f=file-1,r=rank-1; r>=0 && f>=0; --f,--r) if (board[f][r]) return true; // diagonally to top-left
      for(int f=file+1,r=rank-1; r>=0 && f<N ; ++f,--r) if (board[f][r]) return true; // diagonally to top-right
      return false;
    }
    void  PlaceQueenOn(int file, int rank) { board[file][rank] = true ; }
    void RemoveQueenAt(int file, int rank) { board[file][rank] = false; }

  public:
    int CountSolutions() { return Recurse(0); }
  };  
}
```

And our two uncomment tests still pass.

Are we done refactoring? We could change the board array into a class and put the Place/RemoveQueens and IsSpotUnderAttack methods on it. Feel free to do that, if you like.

Let's uncomment a few more tests. On my machine, I can run all the tests up to the 13x13 in less than a second.

The rest start taking longer and longer. Is there a way to speed them up?

Sure:  rather than iterating in the 3 different directions looking for spots with queens on them, we could hold onto 3 arrays of bools that cache that info for us.

But let's save that for a refactoring exercise (I'll add the link as soon as I work up the refactoring kata).


[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)
