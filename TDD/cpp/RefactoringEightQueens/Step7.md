---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 7"
permalink: /TDD/cpp/RefactoringEightQueens/Step7.html
---

After the final relfactoring, to remove the comment and default parameter, results in this:

```cpp

namespace Eight
{
  template <int SIZE> class Queens
  {
    bool board[SIZE][SIZE]={false}, colOccupied[SIZE]={false}, diag1Occupied[SIZE*2 - 1]={false}, diag2Occupied[SIZE*2 - 1]={false};

    int Solve(int row)
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
    int CountSolutions() { return Solve(0); }
  };
}
```

And again, all the tests still pass. 

Notice the tabular-looking code: I really like writing code like this because bugs of some classes of errors just leap off the page at me.

And I think we're done.


[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)  
[C++ Dojo](/TDD/cpp/katas.html)  
