---
layout: page_with_comments
title: "Eight Queens: Step 1"
permalink: /TDD/cpp/EightQueens/Step1.html
---

The Eight Queens Puzzle:  place eight queens on a chessboard in such a way that no queen attacks another.

The standard way to solve these kinds of problems (e.g., a Sudoku solver, Knight's Tour problem, etc.) is with a recursive backtracking algorithm.

A friend back at *StupendousCorp* told me that he was a big believer in TDD, but he was working on compilers which means doing a lot of recursion on abstract syntax trees, and recursion and TDD are a bit difficult to do together.

In TDD, we usually work in little bits, adding more and more functionality as we go along.
With recursion however, once you add that recursive call, it goes and does the whole shebang.

So, we need a way to break the problem down into something small.

For the Eight Queens Puzzle, let's start off with something simpler, like the One Queen Puzzle.
Then we'll work our way up to the 4 queens puzzle and finally the 8 queens puzzle.

### Problem statement:  write code to determine how many solutions there are for N queens on an NxN chessboard.

We can write all the acceptance tests upfront, because [wikipedia has the answers](https://en.wikipedia.org/wiki/Eight_queens_puzzle#Counting_solutions_for_other_sizes_n).

So, let's write a few tests, but comment out all but the first one.
(I'll be using my [module-only test harness](https://github.com/MiddleRaster/tdd20) with my [Visual Studio Test Explorer test adapter](https://github.com/MiddleRaster/Tdd20.TestAdapter) but any test harness will do)

```cpp
import tdd20;
import VsTdd20;
import std;

#include "EightQueens.h"

using namespace TDD20;

VsTest tests[] = {
  {"there is           1 solution  for a  1x1  board", []() { Assert::AreEqual(         1, Eight::Queens< 1>().CountSolutions()); }},
//{"there are          0 solutions for a  2x2  board", []() { Assert::AreEqual(         0, Eight::Queens< 2>().CountSolutions()); }},
//{"there are          0 solutions for a  3x3  board", []() { Assert::AreEqual(         0, Eight::Queens< 3>().CountSolutions()); }},
//{"there are          2 solutions for a  4x4  board", []() { Assert::AreEqual(         2, Eight::Queens< 4>().CountSolutions()); }},
//{"there are         10 solutions for a  5x5  board", []() { Assert::AreEqual(        10, Eight::Queens< 5>().CountSolutions()); }},
//{"there are          4 solutions for a  6x6  board", []() { Assert::AreEqual(         4, Eight::Queens< 6>().CountSolutions()); }},
//{"there are         48 solutions for a  7x7  board", []() { Assert::AreEqual(        40, Eight::Queens< 7>().CountSolutions()); }},
//{"there are         92 solutions for a  8x8  board", []() { Assert::AreEqual(        92, Eight::Queens< 8>().CountSolutions()); }},
//{"there are        352 solutions for a  9x9  board", []() { Assert::AreEqual(       352, Eight::Queens< 9>().CountSolutions()); }},
//{"there are        724 solutions for a 10x10 board", []() { Assert::AreEqual(       724, Eight::Queens<10>().CountSolutions()); }},
//{"there are       2680 solutions for a 11x11 board", []() { Assert::AreEqual(      2680, Eight::Queens<11>().CountSolutions()); }},
//{"there are      14200 solutions for a 12x12 board", []() { Assert::AreEqual(     14200, Eight::Queens<12>().CountSolutions()); }},
//{"there are      73712 solutions for a 13x13 board", []() { Assert::AreEqual(     73712, Eight::Queens<13>().CountSolutions()); }},
//{"there are     365596 solutions for a 14x14 board", []() { Assert::AreEqual(    365596, Eight::Queens<14>().CountSolutions()); }},
//{"there are    2279184 solutions for a 15x15 board", []() { Assert::AreEqual(   2279184, Eight::Queens<15>().CountSolutions()); }},
//{"there are   14772512 solutions for a 16x16 board", []() { Assert::AreEqual(  14772512, Eight::Queens<16>().CountSolutions()); }},
//{"there are   95815104 solutions for a 17x17 board", []() { Assert::AreEqual(  95815104, Eight::Queens<17>().CountSolutions()); }},
//{"there are  666090624 solutions for a 18x18 board", []() { Assert::AreEqual( 666090624, Eight::Queens<18>().CountSolutions()); }},
//{"there are 4968057848 solutions for a 19x19 board", []() { Assert::AreEqual(4968057848, Eight::Queens<19>().CountSolutions()); }},
};
```

Now, let's start TDDing.
Write just enough code to make this test fail. Then, click [Next](Step2.html).
