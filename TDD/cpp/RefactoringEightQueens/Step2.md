---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 2"
permalink: /TDD/cpp/RefactoringEightQueens/Step2.html
---

Ok, at this point, the existing code has been modified just enough to get the tests to pass.
It probably looks something like this:

```cpp
namespace Eight
{
  template <int SIZE>
  class Queens
  {
    int numSolns = 0;
    bool board[SIZE][SIZE]={false}, colOccupied[SIZE]={false}, diag1Occupied[SIZE * 2 - 1]={false}, diag2Occupied[SIZE * 2 - 1]={false};

    void Solve(int row = 0) // assumes all rows before "row" have correctly-placed queens so far
    {
      if (row < SIZE)
      {
        for (int col = 0; col < SIZE; col++)
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
                Solve(row + 1);
                board[row][col] = colOccupied[col] = diag1Occupied[diag1] = diag2Occupied[diag2] = false;
              }
            }
          }
        }
      }
      else
      {
        ++numSolns;
      }
    }

  public:
    int CountSolutions()
    {
      Solve();
      return numSolns;
    }
  };
}
```

At this point, all the tests pass, and we're ready to refactor.

First, I dislike classes with a lot of data-members, and this one has a ton.
We need the ```board``` array and the 3 caches, but we don't need ```numSolns```.

Let's refactor to get rid of that, and then click [Next](Step3.html).
