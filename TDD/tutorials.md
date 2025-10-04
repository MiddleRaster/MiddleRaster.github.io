---
layout: page
title: "TDD Tutorial"
permalink: /TDD/tutorials.html
---

# Test-Driven Development (TDD)


<h4 style="color:red;">Red</h4>
TDD means writing a single fine-grained test before writing the code. 
Then you write just enough source code to make the test **fail**. Why do we make it fail first? 
Because sometimes the test may pass by accident. Seeing it in the failing state and then after writing just enough code to make it pass, that proves that the code is what made the test go green.


<h4 style="color:green;">Green</h4>
When making the test go green, don't be clever, don't make the code pretty (yet), don't optimize it. Just get it to pass.


#### Refactor
Once you've seen the code go from red to green, it's your job to keep it green while you refactor, while you make it clean, clear, pretty. Knowing what direction to factor towards is crucial and requires lots of experience with messy code. Pair with an experienced TDDer and watch what he does.

---

You know you're doing it right if you never fire up the debugger. Debugging is seen as a sign of failure, because you don't understand the code you just wrote.

You quickly accumulate a large test suite, so they must be **blazingly** fast: hundreds of tests per second. Mock out slow dependencies. This large test suite is also a regression suite, so you should never regress functionality while fixing a bug or adding a new feature.

To practice TDD, people have come up with exercises or "Katas." I'll walk you through mine below.

You can do my Katas in either [c++](cpp/katas.html) or [c#](csharp/katas.html).
