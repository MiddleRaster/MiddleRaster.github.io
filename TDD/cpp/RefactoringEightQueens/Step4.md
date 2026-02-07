---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 4"
permalink: /TDD/cpp/RefactoringEightQueens/Step4.html
---

Doing the next refactoring, to get rid of the ```else```, results in code something like this:

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
        if (!colOccupied[col])
        {
          int diag1 = row + col;
          if (!diag1Occupied[diag1])
          {
            int diag2 = SIZE - row + col - 1;
            if (!diag2Occupied[diag2])
            {
              board[row][col] = colOccupied[col] = diag1Occupied[diag1] = diag2Occupied[diag2] = true;
              count += Solve(row + 1);
              board[row][col] = colOccupied[col] = diag1Occupied[diag1] = diag2Occupied[diag2] = false;
            }
          }
        }
      }
      return count;
    }

  public:
    int CountSolutions() { return Solve(); }
  };
}
```

All the tests pass. 

Onto the next refactoring:  I see that the nesting is deeper than I like.
Let's add a method, ```IsSpotUnderAttack```, and see if we can get rid of some of that nesting.

Then click [Next](Step5.html).
