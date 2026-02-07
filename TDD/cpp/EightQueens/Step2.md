---
layout: page_with_comments
title: "Eight Queens: Step 2"
permalink: /TDD/cpp/EightQueens/Step2.html
---

So, writing enough code to make our single uncomment test fail means you probably wrote something like this (since I gave you the signature already):

```cpp
namespace Eight
{
  template <int N>
  class Queens
  {
  public:
    int CountSolutions() { return -1; }
  };  
}
```

Building and running that test does indeed result in the expected error.
The next step is to make the test pass, which we do by returning the proper answer:

```cpp
  int CountSolutions() { return 1; }
```

Compile and test:  passes, as expected. Nothing to refactor.

### Recursive Backtracking Algorithm

At this point, we might as well implement the recursive backtracking algorithm.
They are all of similar form, something like:  (in pseudo-code)

```
int somefunction(int current_level_of_recursion)
{
  if reached max_level_of_recursion, we found a solution:  return 1

  int count = 0
  for all possibilities (in our case, possible valid moves)
    do   the thing (in our case, place a queen)
    count += omefunction(int current_level_of_recursion + 1) // recurse
    undo the thing (in our case, remove the queen)

  return count
}
```

Ok, now that we know sort of what we need to write, let's write it (and make sure the test keeps passing).
Then click [Next](Step3.html).
