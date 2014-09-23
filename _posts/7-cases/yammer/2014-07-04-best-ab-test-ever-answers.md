---
layout: sqlschool-case
categories: cases yammer answers
title:  "The Best AB Test Ever"
date:   2014-07-01 00:00:56
seo-title: 
description: 
---

###Preparation and Prioritizing

A/B tests can alter user behavior in a lot of ways, and sometimes these changes are unexpected. Before digging around test data, it's important to hypothesize how a feature might change user behavior, and why. If you identify changes in the data first, it can be very easy to rationalize why these changes should be obvious, even if you never would have have thought of them before the experiment.

It's similarly important to develop hypotheses for explaining test results before looking further into the data. These hypotheses focus your thinking, provide specific conclusions to validate, and keep you from always concluding that the first potential answer you find is the right one.

For this problem, a number of factors could explain the anomalous test. Here are a few examples:

* **This metric is incorrect or irrelevant:** Posting rates may not be the correct metric for measuring overall success. It describes how Yammer's customers *use* the tool, but not necessarily if they're getting value out of it. For example, while a giant "Post New Message" button would probably increase posting rates, it's likely not a great feature for Yammer. You may want to make sure the test results hold up for other metrics as well.
* **The test was calculated incorrectly**: A/B tests are statistical tests. People calculate results using different methods&mash;sometimes that method is incorrect, and sometimes the arthmetic is done poorly. 
* **The users were treated incorrectly:** Users are supposed to be assigned to test treatments randomly, but sometimes bugs interfere with this process. If users are treated incorrectly, the experiment may not actually be random.
* **There is a confounding factor or interaction effect**: These are the trickiest to identify. Experiment treatments could be affecting the product in some other way&mdash;for example, it could make some other feature harder to find or create incongruous mobile and desktop experiences. These changes might affect user behavior in unexpected ways, or amplify changes beyond what you would typically expect.

<div id="solution"></div>
###Validating the Results

Each step is linked below, but if you want to follow along with all of the charts in one view, [check them out here](https://modeanalytics.com/modeanalytics/lists/665647b40bb0/runs/307f7300be05).

**1.** The number of messages sent shouldn't be the only determinant of this test's success, so dig into a few other metrics to make sure that their outcomes were also positive. In particular, we're interested in metrics that determine if a user is getting value out of Yammer. (Yammer typically uses login frequency as a core value metric.)

First, the average number of logins per user is up. This suggests that not only are users sending more messages, but they're also signing in to Yammer more.

<a href="https://modeanalytics.com/benn/reports/ff3bdfe7f1ef/runs/e3dcd3a14b75/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

Second, users are logging in on more days as well (days engaged the distinct number of days customers use Yammer). If this metric were flat and logins were up, it might imply that people were logging in and logging out in quick succession, which could mean the new feature introduced a login bug. But both metrics are up, so it appears that the problem with this tests isn't cherry-picking metrics&mdash;things look good across the board.

<a href="https://modeanalytics.com/benn/reports/9a0426b46f22/runs/efebd36c1884/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**2.** Reasonable people can debate which mathematical methods are best for an A/B test, and arguments can be made for some changes (1-tailed vs. 2-tailed tests, required sample sizes, assumptions about sample distributions, etc.). Nontheless, these other methods don't materially affect the test results here. For more on the math behind A/B testing, this [brief Amazon primer](https://developer.amazon.com/sdk/ab-testing/reference/ab-math.html) offers good information, as does [Evan Miller's blog](http://www.evanmiller.org/index.html).

The test, however, does suffer from a methodological error. The test lumps new users and existing users into the same group, and measures the number of messages they post during the testing window. This means that a user who signed up in January would be considered the same way as a user who signed up a day before the test ended, even though the second user has much less time to post messages. It would make more sense to consider new and existing users separately. Not only does this make comparing magnitudes more appropriate, be it also lets you test for novelty effects. Users familiar with Yammer might try out a new feature just because it's new, temporarily boosting their overall engagement. For new users, the feature isn't "new," so they're much less likely to use it just because it's different.

**3.** Investigating user treatments (or splitting users out into new and existing cohorts) reveals the heart of the problem&mdash;all new users were treated in the control group.

<a href="https://modeanalytics.com/benn/reports/4a83b254000f/runs/637790980c2e/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

This creates a number of problems for the test. Because these users have less time to post than existing users, all else equal, they would be expected to post less than existing users given their shorter exposure to Yammer. Including all of them in the control group lowers that group's overall posting rate. Because of this error, you may want to analyze the test in a way that ignores new users. As you can see from below, when only looking at posts from new users, the test results narrow considerably.

<a href="https://modeanalytics.com/benn/reports/50e7b028a56b/runs/5a37269eefd0/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**4.** Though we've identified one problem, it's usually a good idea to explore other possibilities as well. There can be multiple problems, and frequently, these problems are related. When recommending what to next to product teams, it's important to have a complete understanding of what happened during a test&mdash;both the expected results and unexpected ones.

Interaction effects can appear in many ways, so it's not possible to list all of the possibilities here. However, a few cohorts&mdash;new vs. existing users, usage by device, usage by user type (i.e., content producers vs. readers)&mdash;are usually good to check. 

The graph below show that the test results for just messages sent on phones. If you click through the embed link, you can update the report to see results by tablet and desktop devices as well.

<a href="https://modeanalytics.com/modeanalytics/reports/a27b4b41da8b/runs/43337ed65dca/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

###Follow Through
Overall, the test results are still strong. But given the above result, we should validate that change across different cohorts and fix the logging error that treated all new users in one group.