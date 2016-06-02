---
layout: post
title:  "Impact of History and Bookmarks on Engagement"
date:   2016-06-02 17:31:52 -0400
categories: graphs
---

### Question
Are the number of bookmarks or history items associated with how many days a user returns to activity stream over the past 30 days?

For now, let's assume that engaged users are those returning more than the 3rd quantile number of days within the last 30 days.

First, we measure the third quantile over days visited for all users; it's 3 days.

Let's say engaged users are those who have interacted with Activity Stream for 3 or more days over the past 30 days.

<iframe width="900" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/8.embed"></iframe>

### Conclusions
* Pearson's correlation coefficient for bookmarks vs. days visited is **-0.0015**. This is very small and shows basically no correlation
* Pearson's correlation coefficient for history vs. days visited is **0.088**. This is still quite small but as seen in the graph, it's significant enough to account for all the blue dots that have fewer history items and aren't overshadowed by orange dots.


