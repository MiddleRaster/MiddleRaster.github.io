---
layout: page_with_comments
title: "Refactoring Eight Queens: Step 1"
permalink: /TDD/cpp/RefactoringEightQueens/Step1.html
---

Here's a refactoring kata.

Background:  once at *StupendousCorp*, I gave my TDD class to a neighboring team. Later, the lead for that team found himself on a 12-hour flight and he decided to implement the [Eight Queens Puzzle](/TDD/cpp/EightQueens/Step1.html).
Unfortunately, he did not do TDD (as he'd only just learned about it), but he came up with the following code:

```cpp
#include "stdafx.h"
#include <iostream>
 
#define SIZE 8
 
int numSolns;
bool board[SIZE][SIZE], colOccupied[SIZE], diag1Occupied[SIZE * 2 - 1], diag2Occupied[SIZE * 2 - 1];
 
std::ostream& operator<<(std::ostream &out, bool board[SIZE][SIZE])
{
    for (int row = 0; row < SIZE; row++)
    {
        for (int col = 0; col < SIZE; col++)
            out << (board[row][col] ? "Q " : ". ");
 
        out << std::endl;
    }
    return out;
}
 
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
        std::cout << "Solution # " << ++numSolns << ":" << std::endl << board << std::endl;
    }
}

 
void _tmain(int argc, _TCHAR* argv[])
{
    Solve();
}
```

Now, the code contains a good idea:  to see if a spot on the chessboard is under attack, rather than iterating over the positions in the vertical and two diagonal directions, he kept that information cached.
And that makes the code much faster.

However, it certainly isn't up to my standards.
But we have all the tests from the [Eight Queens Puzzle kata](/TDD/cpp/EightQueens/Step1.html) that we can use to be sure that our refactoring hasn't broken anything.

So, set up a project using those tests and the code above, and get them to build. Getting the tests to run means making a few changes already:
1. rename _tmain and have it return the number of solutions found.
2. Wrap the whole thing in a template class, using SIZE as the template parameter name.
3. remove all the std::ostream stuff (probably put in there for debugging), since we only want to return the count


Once you get everything building and the tests passing, we'll be ready to refactor. Then, click [Next](Step2.html).
