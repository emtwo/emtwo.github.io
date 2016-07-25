---
layout: post
title:  "Activty Stream Engagement Metrics - Part 1"
categories: graphs
---

## Goal

In this post we'll be exploring some common existing web engagement metrics as defined in the book *"Measuring the Immesurable: Visitor Engagement"* [[1]](#references)

First, we'll be computing 5 of the metrics described there for individual users:

* Interaction Index (without block and delete)
* Negative Interaction Index (only block and delete)
* Session Duration Index
* Recency Index
* Loyalty Index

Then we'll define an engaged user as one that returns and interacts with Activty Stream at least once per week for at least the past 3 consecutive weeks.

We'll then try to build a model that can predict an engaged user given their behaviour (the 45metrics).

Finally, we'll talk about what can be done with these results and what further exploration is left for the future.

<br>

## Engagement Metrics

### 1. Interaction Index Distribution

The following graph shows a distribution of interaction index for all of our users. In this case, we compute interaction rate for each user as:

*<center>number of sessions with at least one (non-block and non-delete) interaction / total number of user sessions * 100</center>*

<iframe width="800" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/67.embed"></iframe>

**NOTES:**

* The chart generally trends downward showing fewer very dedicated users with high interaction rates
* We can see a peak (~15% of users) with between 5% to 10% of sessions having interactions 
* The peak at 100% is likely from users with only a few sessions overall

<br>
<br>
<br>

### 2. Negative Interaction Index Distribution

The following graph shows a distribution of negative interaction index for all of our users. In this case, we compute interaction rate for each user as:

*<center>number of sessions with at least one (block or delete) interaction / total number of user sessions * 100</center>*

<iframe width="800" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/78.embed"></iframe>

**NOTES:**

* A vast majority of users (~62%) have a 0% to 5% interaction rate with a decaying trend
* It's probable that users are liking what they're seeing if they're clicking more than blocking/deleting

<br>
<br>
<br>

### 3. Session Duration Index Distribution

The following graph shows a distribution of session duration index for all of our users. First we choose the overall median session duration (5.5 seconds) to be the threshold. Then we compute each user's session duration index as:

*<center>number of sessions with duration exceeding the threshold / total number of user sessions * 100</center>*

<iframe width="800" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/63.embed"></iframe>

**NOTES:**

* The shape of the chart (centre peak) is likely due to the choice of median as a threshold
* The peak at 100% is likely once again influenced by users with a small number of sessions. It could also be from users who frequently leave their tabs open and idling in this case.

<br>
<br>
<br>

### 4. Recency Index Distribution

The following graph shows a distribution of recency index for all of our users. The recency index is computed for a given visit date as follows:

*<center>1 / number of days elapsed since the most recent session * 100</center>*

and is then averaged over the total number of days visited.

<iframe width="800" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/69.embed"></iframe>

**NOTES:**

* The peak at 0% accounts for both users who have a total of 1 visit only and those who have very sparse/distributed visits.
* There's also a little peak at 50% which means visiting every other day seems to be common

<br>
<br>
<br>

### 5. Loyalty Index Distribution

The following graph shows a distribution of loyalty index for all of our users. The loyalty index is computed for each user as follows:

*<center>1 - (1 / number of sessions over the past 3 weeks) * 100</center>*

<iframe width="800" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/74.embed"></iframe>

**NOTES:**

* ~54.3% of all users did not open activity stream at all or only opened it once over the past 3 weeks.
* ~30% of all users returned over the past 3 weeks and were *very* loyal with *at least* 20 visits total
* Given the nature of the computation, it's not possible to have a loyalty index greater than 0 and less than 50.

<br>
<br>
<br>

### Analyzing Correlations Between Metrics

Now that we've seen an overview of user behaviour, we can try to identify some relationships between behaviours to better understand our users. **Table 1** below shows the Pearson correlation coefficient [[2]](#references) for all combinations of our 5 metrics above.

When the coefficient is close to 0 it implies no correlation, when it's close to one it implies a positive correlation, and close to -1 implies a negative correlation.

#### Table 1. Correlations Between Metrics

|  Metric 1 | Metric 2 | User Type | Correlation |
|-----------|----------|-------------|-----------|
| Interaction Index | Session Duration Index | Unengaged | 0.430325647134 |
| Interaction Index | Session Duration Index | Engaged | 0.392083132159 |
| Interaction Index | Recency Index | Engaged | 0.34084020437 |
| Interaction Index | Recency Index | Unengaged | 0.25021778555 |
| Loyalty Index | Recency Index | Engaged | 0.24676779249 |
| Interaction Index | Negative Interaction Index | Engaged | 0.113349350414 |
| Interaction Index | Negative Interaction Index | Unengaged | 0.108766867896 |
| Session Duration Index | Negative Interaction Index | Engaged | 0.104265768653 |
| Session Duration Index | Negative Interaction Index | Engaged | 0.0876997159773 |
| Loyalty Index | Recency Index | Unengaged | 0.0847442251109 |
| Recency Index | Session Duration Index | Unengaged | 0.0557225952733 |
| Recency Index | Negative Interaction Index | Unengaged | 0.0277618916522 |
| Recency Index | Negative Interaction Index | Engaged | 0.0273987012206 |
| Recency Index | Session Duration Index | Engaged | -0.0231654117681 |
| Loyalty Index | Negative Interaction Index | Engaged | -0.0409611062983 |
| Loyalty Index | Negative Interaction Index | Unengaged | -0.0627722736047 |
| Loyalty Index | Session Duration Index | Unengaged | -0.182615114018 |
| Loyalty Index | Session Duration Index | Engaged | -0.201243275844 |
| Interaction Index | Loyalty Index | Engaged | -0.237541403076 |
| Interaction Index | Loyalty Index | Unengaged | -0.28686159045 |

<br>
<br>

#### Given these correlations, we can make some speculations about the users' behaviour:

* Users who have higher interaction rates also have higher session durations
  * this is expected since it takes time to browse around and click, we can almost call it a sanity check

* High recency index and high interaction index is also correlated
  * users who come back frequently are more likely to interact or vice versa  

* Users with high loyalty have low interaction and session duration
  * this one is more interesting - these users have many short, non-interactive visits - they probably just open a new tab and quickly navigate away

* Engaged users who have high loyalty also have high recency
  * Despite the label "recency index", it's a bit of a misnomer. Unengaged users may not have returned recently and this isn't taken into account in the recency index but it is in the loyalty index

<br>
<br>
<br>

### Metric Averages

We can also take a look at some metric averages for engaged vs. unengaged in **Table 2** to see how differently these users behave.


#### Table 2. Metric Averages for Engaged vs. Unengaged Users

|                            | Engaged | Unengaged |
|----------------------------|---------|-----------|
| Interaction Index          | 36.34   | 19.64     |
| Negative Interaction Index | 0.90    | 0.87      |
| Session Duration Index     | 64.46   | 61.40     |
| Loyalty Index              | 89.21   | 50.04     |
| Recency Index              | 73.93   | 33.92     |

<br>

As expected, all averages are higher for engaged users. The most drastic being loyalty and recency.

The interaction index average for unengaged users is still fairly high though.

<br>
<br>
<br>

## Defining Engaged Users

Now that we've looked at various user behaviours that tell us a little bit about engagement, let's define our ultimate engagement goal as bringing users back and interacting with Activity Stream at least once per week.

We'll call a user "engaged" if they've interacted with Activity Stream at least once per week over the past 3 weeks and we'll look at the ratio of engaged vs. unengaged over the past 8 weeks in the graph below.

<iframe width="800" height="500" frameborder="0" scrolling="no" src="https://plot.ly/~emtwo/77.embed"></iframe>

**NOTES:**

* This is cool! It seems we've been keeping a (roughly) steady number of engaged users over the weeks, but our drop in overall users is mostly comning from the engaged users.

<br>
<br>
<br>

## Classifying Engaged Users

We've defined user behaviours and labels for engaged vs. unengaged. With this we have enough information to design a classifier. But let's talk about the motivation first.

<br>

### Motivation for an Engagement Classifier

In theory, if we can predict whether a user will be engaged given their behaviour, we can try to probe certain behaviours for them to become more engaged.

For example, if we find that users who return at least 3 times a week also return every week then we can put more effort into making users interact more times per week, such as having fresh, frequent, relevant recommendations.

On the other hand, if no such relationship exists, the motivation to make users interact more times per week is lower.

So let's take a look at some basic classifier performance in **Table 3**:

#### Table 3. Classifier Performance

|           | Classification Method | Precision       | Recall         |
|-----------|-----------------------|-----------------|----------------|
| Engaged   | Logistic Regression   | 0.0935817202736 | 0.996899224806 |
| Unengaged | Logistic Regression   | 0.999413833529  | 0.353807843951 |
| Engaged   | SVM                   | 0.185929648241  | 0.458914728682 |
| Unengaged | SVM                   | 0.959843516281  | 0.865532268105 |
| Engaged   | Decision Tree         | 0.249037227214  | 0.902325581395 |
| Unengaged | Decision Tree         | 0.992071482507  | 0.817908279726 |
| Engaged   | Random Forest         | 0.274735830932  | 0.886821705426 |
| Unengaged | Random Forest         | 0.991098646507  | 0.843328491388 | 

<br>

Cool! Our best performing classifier is, unsurprisingly, the Random Forest. It performs reasonably well given these metrics except for engaged user prediction. This means that while 88% of the engaged validation set is is correctly classified, only 27% of the users classified as engaged were indeed engaged. The rest were unengaged.

This doesn't sound very good, but generally there is a tradeoff between precision and recall and if precision were more important for us we could take a hit on recall and increase precision.

For the purpose of this trial though, we are just trying to see if it's even **possible** to get reasonable classification results, but we haven't thoroughly desgined a solid classifier yet. And indeed it does seem possible!

<br>
<br>
<br>

## Conclusions

There are many observations and conclusions that can be drawn from this analysis. The three most important outcomes are:

1. Getting a better understanding of our user behaviour

2. Seeing that our defintion of engaged users returning for several consecutive weeks is reasonable (though it could be improved).

3. Seeing that there is a strong distinction between engaged and unengaged users

<br>
<br>
<br>

## Next Steps

1. **Improving our definition of "engaged"**. Perhaps one of the reasons engaged precision is low is that our definition for engagement is not quite right. Users might be "engaged" visually which shows up as a longer session duration, high loyalty and high recency but not as much of an interaction rate. The unengaged users also seem to have a fairly high interaction index average (almost 20%!) This idea is worth exploring further.

2. **Cluster users into various types by behaviour**. As we saw, some users open many new tabs for a short time but don't interact, some interacted a lot and then stopped, some interact very regularly. Understanding our user types can help us make a better experience for those who seem to lose interest.

3. **Improving our classification accuracy**. There is a lot that can be done here. Getting or generating more training data, adding more features (e.g. history size, number of bookmarks, user's OS, etc), tuning hyperparameters, experimenting with various ensemble methods, and so on.

<br>
<br>

{: id="references"}
## References

[1] [E. Peterson and J. Carrabis, Measuring the Immeasurable: Visitor Engagement. Web Analytics Demystified, 2008.](http://www.webanalyticsdemystified.com/downloads/Web_Analytics_Demystified_and_NextStage_Global_-_Measuring_the_Immeasurable_-_Visitor_Engagement.pdf)

[2] [Pearson's Coefficient Correlation](https://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient)

[3] [IPython Notebook with Analysis](https://gist.github.com/emtwo/106e9b6701f17f02ce6323ee97bcb686)