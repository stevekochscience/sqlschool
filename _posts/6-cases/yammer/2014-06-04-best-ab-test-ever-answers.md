---
layout: sqlschool-case
categories: cases yammer answers
title:  "The Best AB Test Ever"
date:   2014-06-01 00:00:56
seo-title: 
description: 
---

###Preparation and Prioritizing
* This metric is incorrect or irrelevant - usage vs value metrics
* The users were treated incorrectly
* The test was calculated incorrectly
* There is a confounding factor or interaction effect

<div id="solution"></div>
###Validating the Results

Each step is linked, below, but if you want to follow along with all of the charts in one view, [check them out here](https://modeanalytics.com/modeanalytics/lists/665647b40bb0/runs/307f7300be05).

**1.** The easiest place to start here is to look at the performance of a few other metrics. The number of messages sent shouldn't be the only determinant of this test's success, so it's worth digging into a few other big ones to make sure that their outcomes were also positive. In particular, we're interested in "value" metrics (see above).

First up, the number of days that users engaged. It appears to show a similar lift to the Messages Sent test:

<a href="https://modeanalytics.com/benn/reports/9a0426b46f22/runs/efebd36c1884/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

Logins are up as well, so it looks there isn't a problem with cherry-picking a metrics that looks especially good &mdash; this test looks great all-around:

<a href="https://modeanalytics.com/benn/reports/ff3bdfe7f1ef/runs/e3dcd3a14b75/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**2.** There is a problem, though: the test groups all users together regardless of how long they have used the product. There isn't something we can show graphically, so you'll just have to take our word for it. It would make more sense to test returning users separately from, say, users who activated in the most recent week. This is particularly relevant for measuring the magnitude of messages posted &mdash; new and returning users are likely to post messages in different volumes.

**3.** treating new users and existing users the same is problematic anyway, since there may be novelty effects for existing users.

**4.** we should make sure the change didn't have a disproportionate affect on different devices.

--SHOW TEST BY DEVICE TYPE

**5.** Finally, the test was run incorrectly. All new users were treated in the control group. Because these users, on average, post less than existing users given their shorter exposure to Yammer, including all of them in the control group lowers that group's overall posting rate. Because of this error, we should throw out new users from this test.

treatments by activation month
<a href="https://modeanalytics.com/benn/reports/4a83b254000f/runs/637790980c2e/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

###Follow Through
Overall, the test results are strong. But given the above result, we should validate that change across different cohorts and fix the logging error that treated all new users in one group.
