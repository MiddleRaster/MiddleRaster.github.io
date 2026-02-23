---
layout: page_with_comments
title: "Historical ABA Charts"
permalink: /ABA/HistoricalABACharts.html
---

What do Historical ABA Charts look like and what can we learn from them?  
Here are some examples.

In each image, the blue line and axis is the average age of open bugs (ABA).  
The orange line and axis is the number of open bugs.  
The horizontal axis is, of course, the date.

# Microsoft VS Code

![Alt text](./MicrosoftVscodeAverageOpenBugChart.png)

An interesting chart: it looks like this team had aa yearly effort to reduce the number of open bugs, but they're not particularly successful.
Over the course of 10 years, they've accumulated over 6000 open bugs, most of which are probably useless.

I applaud their effort to reduce their bug count, but even the ones they did close didn't change the ABA very much.  
Conclusion:  they're fixing or closing recent bugs only.

And that means that their backlog of bugs is just swamped with ancient issues that will probably never get fixed; these should just be closed, as they're only noise.

Two more things:
1.  For the first four years, they kind of kept a handle on things (a litte), as they kept their open bug count around a thousand (way too high, in my opinion), but then they completely fell off the wagon. Their bug count started going up and it's going up steeper and steeper - a classic sign that they've accumulated more technical debt than they can handle.
2.  They clearly have never heard of the ABA metric, as right from the get-go their ABA started climbing and is showing no sign of slowing down. The average bug is still open after ~600 days, and that can't be making their customers happy.

# Microsoft VS Code

![Alt text](./MicrosoftTypeScriptAverageOpenBugAgeChart.png)

A similar, but significantly worse chart from a team that's only 1 year older than the VS Code team.

I see no yearly effort to reduce bugs and their ABA just rises steadily.  
However, two years ago and again just recently, there was a big push to close some bugs, which I applaud, but look at the first effort:  it actually made the ABA go up. 
That means that they were closing only recent bugs, a common refrain at Microsoft, evidently.  

And look at the second push:  even though their bug count dropped by almost half, their ABA held steady at 1600 days (that's over 4 years)!  
They'll never fix those bugs and are just dragging them along for the ride.

# Zephyr RTOS

![Alt text](./ZephyrAverageOpenBugAgeChart.png)







Back to [ABA Index](./index.html).
Back to [MiddleRaster's pages](https://middleraster.github.io/).
