---
layout: page_with_comments
title: "Bowling Game Kata"
permalink: /TDD/cpp/BowlingGame/BowlingGame.html
---

This is the venerable Bowling Game Kata which is all about scoring bowling.

In case, you're unfamiliar with bowling, here's how it works:
- 10 bowling pins are set up by machine at the start of what is called a **frame**.
- The player bowls the bowling ball, attempting to knock down as many pins as possible.
- If he (or she (we'll use 'he' since it's less typing)) knocks down all 10 pins, it's called a **strike** and he gets 10 points, plus whatever number of pins he knocks down on his next **two** bonus rolls.
- If he doesn't knock them all down on the first try, he gets another roll. If he knocks down all the remaining pins, it's called a **spare** and he gets 10 points, plus whatever number of pins he knocks down on his next  **one** bonus roll.
- If he doesn't knock them all down by his second roll, he gets **zero** bonus rolls, but does get **1 point per pin** that he did knock down.
- That constitutes a frame. And there are 10 of them in a game.
- However, if he rolled a strike in the **last** frame, he gets to roll 2 more times.
- Similarly, if he rolled a spare in the **last** frame, he gets to roll 1 more time.


Now, there are many ways to implement this scoring algorithm cleanly. When I usually do it, my sensibilities lead me to use an array of Frame objects.
Other people like the array of ints way, but I don't like it because it puts more of a mental burden on the reader of the code, because there's no clear boundary between frames.
Finally, a very natural way to think about bowling is to think of it as a state machine, which can be implemented in several different ways.


In any case, select which implementation of the Bowling Game Kata you'd like to do:
 - [Array of Frames](ArrayOfFrames/Step1.html)
 - [Array of Ints](ArrayOfInts/Step1.html)
 - [State Machine](StateMachine/Step1.html)
