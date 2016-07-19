---
layout: post
title:  "[A/B Test 1]: Delete vs. Dismiss"
categories: graphs
---

Analysis by: [Nan Jiang](https://github.com/ncloudioj)  
Review & edit by: [Marina Samuel](https://github.com/emtwo)

<br>


## Experiment Goal ##

The Activity Stream team has been working on an A/B test infrastructure and our first experiment was to identify whether users knows the difference between "dismiss" and "delete" in the context menu. In **Figure 1** we see the original context menu prior to the experiment.

<br>

**<center>Figure 1: Original Context Menu</center>**

<center><img src="{{ post.url }}/assets/context_menu.png" alt="Activity Stream" height="200" width="300"></center>

<br>

## Experiment Design ##

50% of the Activity Stream population was assigned as the control group and kept the original context menu **(Figure 1)**

The other 50% of the population was the experiment group and had the reverse order of "Dismiss" and "Delete"

<br>

## Statistical Analysis
We use the python implementation [[1]](#references) for both an independent two-sample t-test [[2]](#references) and Welch's t-test [[3]](#references) on the data below with the following null hypothesis:

*<center>"The average number of unique users who have clicked on the "delete" and "dismiss" buttons are the same in both the control and experiment group".</center>*

### Table 1. Delete

| Group      | Unique users by date (2016-06-12 to 2016-06-21)                   | Total unique users        |
|---------   + ----------------------------------------------------------------- |-------------------------  |
| control    | 142, 174, 145, 204, 185, 187, 161, 139, 180, 113                  | 1126                      |
| experiment | 140, 160, 189, 202, 220, 167, 148, 133, 182, 118                  | 1062                      |

<br>

### Table 2. Dismiss

| Group      | Unique users by date (2016-06-12 to 2016-06-21)                   | Total unique users        |
|---------   + ----------------------------------------------------------------- |-------------------------  |
| control    | 90, 121, 142, 179, 174, 138, 100, 97, 142, 113 | 905 |
| experiment | 90, 116, 97, 147, 159, 129, 116, 85, 111, 76 | 781 |

<br>

## Results

### Table 3. P-values

|                         |        Delete       |       Dismiss       |
|-------------------------|---------------------|---------------------|
|Two-sample t-test| 0.83242918011910816 | 0.20632972060335547 |
|Welch's t-test   | 0.83248161658486686 | 0.20666033966856817 |

<br>

## Further Analysis

We further want to analyze whether both the delete and dismiss options are valuable to users. We can perform another t-test with the following null hypothesis:

*<center>"The average number of unique users who have clicked on the "delete" and "dismiss" buttons is the same".</center>*


### Table 4. Delete vs. Dismiss

|  Event  |  Unique users by date (2016-06-12 to 2016-06-21)      | Total unique users | 
|---------|-------------------------------------------------------|-------| 
| delete  | 282, 334, 334, 406, 405, 354, 309, 272, 362, 231      | 2188 | 
| dismiss | 180, 237, 239, 326, 333, 267, 216, 182, 253, 189      | 1686 |  

<br>

Here the p-value is 0.003 < *α*(5%) so we can reject the null hypothesis. We further note that 781 out of 3093 (25.2%) users have clicked on both options in the experiment

<br>

## Conclusions

1. Since both t-tests have nearly identical p-values for both delete and dismiss, we know that the variance in the control and experiment group is almost the same.

2. Since both the delete and dismiss p-values in **Table 3** are larger than 0.1, we cannot reject the null hypothesis. We conclude that regardless of the position of the "delete" and "dismiss" options, users know which one they want to use.

3. Users are significantly more likely to use "delete" than "dismiss" but many users (25.2%) have used both and both are useful to keep.

<br>

{: id="references"}
## References

[1] [http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html](http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html)  
[2] [https://en.wikipedia.org/wiki/Student's_t-test](https://en.wikipedia.org/wiki/Student%27s_t-test#Independent_two-sample_t-test)    
[3] [https://en.wikipedia.org/wiki/Welch's_t-test](https://en.wikipedia.org/wiki/Welch%27s_t-test)

<br>

## Github Links

* [Experiment Pull Request](https://github.com/mozilla/activity-stream/pull/777)
* [Experiment Issue](https://github.com/mozilla/activity-stream/issues/732)
* [Original Experiment Analysis](https://github.com/mozilla/activity-stream/issues/864#issuecomment-227551295)
