---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 6"
permalink: /TDD/cpp/RefactoringEightQueens/Step6.html
---

Doing the next refactoring, adding ```PlaceQueen``` and ```RemoveQueen``` methods results in something like:

```cpp

namespace Eight
{
  template <int SIZE> class Queens
  {
    bool board[SIZE][SIZE]={false}, colOccupied[SIZE]={false}, diag1Occupied[SIZE*2-1]={false}, diag2Occupied[SIZE*2-1]={false};

    int Solve(int row = 0) // assumes all rows before "row" have correctly-placed queens so far
    {
      if (row >= SIZE)
        return 1;

      int count = 0;
      for (int col=0; col<SIZE; col++)
      {
        if (IsSpotUnderAttack(row, col))
          continue;

        PlaceQueen(row, col);
        count += Solve(row + 1);
        RemoveQueen(row, col);
      }
      return count;
    }
    bool IsSpotUnderAttack(int row, int col) const { return colOccupied[col] || diag1Occupied[row+col] || diag2Occupied[SIZE-row+col-1]; }
    void PlaceQueen (int row, int col)  { board[row][col] = colOccupied[col]  = diag1Occupied[row+col]  = diag2Occupied[SIZE-row+col-1] = true; }
    void RemoveQueen(int row, int col)  { board[row][col] = colOccupied[col]  = diag1Occupied[row+col]  = diag2Occupied[SIZE-row+col-1] = false; }
  public:
    int CountSolutions() { return Solve(); }
  };
}
```

And again, all the tests still pass. 

Let's get rid of the comment and while we're at it, the default parameter on the ```Solve``` method, since there's really no need for it.

When that's done, click [Next](Step7.html).
