---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 3"
permalink: /TDD/cpp/RefactoringEightQueens/Step3.html
---

Doing that first refactoring, that is, removing ```numSolns```, results in code something like this:

```cpp
namespace Eight
{
  template <int SIZE> class Queens
  {
    bool board[SIZE][SIZE]={false}, colOccupied[SIZE]={false}, diag1Occupied[SIZE*2 - 1]={false}, diag2Occupied[SIZE*2 - 1]={false};

    int Solve(int row = 0) // assumes all rows before "row" have correctly-placed queens so far
    {
      if (row < SIZE)
      {
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
      else
      {
        return 1;
      }
    }

  public:
    int CountSolutions() { return Solve(); }
  };
}
```

The tests still pass.

What next? Well, one of the rules found in Jeff Bay's "[Object Calisthenics](https://williamdurand.fr/2013/06/03/object-calisthenics/)" document says you can use an ```if``` but not an ```else```.
It took me a year to figure out why that was a good idea, but let's go with Bay's idea right now.

Let's refactor to get rid of that ```else```, and then click [Next](Step4.html).
