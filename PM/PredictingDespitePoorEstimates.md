---
layout: page_with_comments
title: "Good Predictions Despite Poor Estimates"
permalink: /PM/PredictingDespitePoorEstimates.html
---

There have been many articles published that purport to explain why our software estimating skills are so poor, but I think they miss the point.  Here's what I think, in 5 parts.

## 1. The Five Orders of Ignorance

In the [October, 2000 issue](https://www.la-acm.org/Archives/laacm0512-Article%2002%20The%205%20Orders%20of%20Ignorance%20OCT%202000.pdf) of the "Communications of the ACM," Philip G. Armour explains what's really going on in software estimation.
Here we go:

- **0th Order Ignorance - "Lack of Ignorance":**  this is when you know everything and life is pretty good when you know everything. In software, we rarely write the same code over and over again, so we're not often in this position. However you could be, if you were doing a port to an operating system that you've ported to before and therefore know everything.

- **1st Order Ignorance - "Lack of Knowledge":**  this one isn't so bad; it's when you realize that you don't know something. Your course of action is clear:  ask someone, do research, experiment, etc., until you do know the answer. If you are asked to give an estimate when you're in this state, you will naturally include time to do that research - don't skimp!

- **2nd Order Ignorance - "Lack of Awareness":**  this is when we start getting into trouble. We don't even realize that we don't know something. These are the "unknown unknowns" that Donald Rumsfeld so famously stated on February 12, 2002. We can't estimate them. We don't even know they exist. If your project management skills are limited to adding up all the estimates (for those items that you do know exist) and using the total to predict when you'll be done, you're going to be in dire straights, because you just gave all those "unknown unknowns" an estimate of 0. And that will not go well, because you gave yourself no time to do all those items. This guarantees schedule slippage as those "unknown unknowns" almost always end up being really important.

-  **3rd Order Ignorance - "Lack of Process":**  in 2nd Order Ignorance, when you start building the software you start discovering all those things you didn't even know existed, but in 3rd Order Ignorance, that process fails. Typically you'll build software that no customer wants. But then they'll tell you, so you do find out eventually, assuming you're still in business.

-  **4th Order Ignorance - "Meta-Ignorance":**  How much worse can it get?  You have 4th Order Ignorance when you don't know about the Five Orders of Ignorance. Now you do. I used to think this was a joke on Armour's part, but it's not: you have been presented with an opportunity. The question now is, how can you make use of this opportunity?

The rest of this post will be about what to do about 2nd Order Ignorance. In the future, I'll have another post on how to fix 3rd Order Ignorance.

## 2. How to Measure 2nd Order Ignorance

Now, you can't measure how many "unknown unknowns" there are (because they're invisible to us), but what you can do is measure the **_rate_** at which these new items are being discovered. 
I do it with a simple spreadsheet that generates a stacked bar chart, something like this:

![Alt text](./BurnUpChart.png)

There's a stacked bar for each week. The green bars' height represents work that has been fully completed, while the height of the red bars represents how much work is yet to be done.
Any newly discovered work raises the height of the red bar. A regression line through the tops of the red bars is the **_rate of discovery_** of the "unknown unknowns," while a line through the tops of the green bars is your team's **_velocity_**.
A couple of things should be immediately obvious:
- If these two lines are parallel or, worse, diverge, then you'll never be done. You probably can't do much about the rate of discovery, but you can increase the team's velocity. **_Don't_** just hire more people: that'll slow you down even more. What to do is another future post.

- If you don't track like this, it's like saying that the rate of discovery line is absolutely flat. Look at where the horizontal line from top of the left-most red bar intersects with the team velocity line (at about 4/22), and compare that to the intersection point of the velocity and rate of discovery lines (slightly past 6/17), which is our best prediction:  the former is way off. **_The completion date is heavily dependent on the slope of the rate of discovery line_**. In fact, in my experience, the slope of the discovery line completely swamps how bad your estimates are, but more on this below.

## 3. How to Create the Burn-up Chart

## 4. How to Use the Burn-up Chart
how to stay on track, how to remove items
when will story X be done. Cross-team dependencies.

## 5. #NoEstimates

