---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 5"
permalink: /TDD/cpp/RefactoringEightQueens/Step5.html
---

Doing the next refactoring, adding an ```IsSpotUnderAtatck``` method to get rid of some nesting, results in something like:

```cpp
namespace Eight
{
  template <int SIZE> class Queens
  {
    bool board[SIZE][SIZE]={false}, colOccupied[SIZE]={false}, diag1Occupied[SIZE*2 - 1]={false}, diag2Occupied[SIZE*2 - 1]={false};

    int Solve(int row = 0) // assumes all rows before "row" have correctly-placed queens so far
    {
      if (row >= SIZE)
        return 1;

      int count = 0;
      for (int col=0; col<SIZE; col++)
      {
        if (IsSpotUnderAttack(row, col))
          continue;

        int diag1 = row + col;
        int diag2 = SIZE - row + col - 1;
        board[row][col] = colOccupied[col] = diag1Occupied[diag1] = diag2Occupied[diag2] = true;
        count += Solve(row + 1);
        board[row][col] = colOccupied[col] = diag1Occupied[diag1] = diag2Occupied[diag2] = false;
      }
      return count;
    }
    bool IsSpotUnderAttack(int row, int col) const
    {
      return colOccupied[col] || diag1Occupied[row+col] || diag2Occupied[SIZE-row+col-1];
    }

  public:
    int CountSolutions() { return Solve(); }
  };
}
```

All the tests still pass. 

Next, let's add ```PlaceQueen``` and ```RemoveQueen``` methods, which will clarify the code because we won't be looking at the ```diag1``` and ```diag2``` calculations.

When that's done, click [Next](Step6.html).
