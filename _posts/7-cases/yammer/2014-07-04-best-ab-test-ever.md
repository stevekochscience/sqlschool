---
layout: sqlschool-case
categories: cases yammer
title:  "The Best A/B Test Ever"
date:   2014-07-01 00:00:56
seo-title: 
description: 
---

Yammer not only develops new features, but is continuously looking for ways to improving existing ones. Like many software companies, Yammer frequently tests these features before releasing them to all of thier customers. These [A/B tests](http://en.wikipedia.org/wiki/A/B_testing) help analysts and product managers better understand a feature's effect on user behavior and the overall user experience.

This case focuses on an improvement to Yammer's core “publisher”&mdash;the module at the top of a Yammer feed where users type their messages. To test this feature, the product team ran an A/B test from June 1 through June 30. During this period, some users who logged into Yammer were shown the old version of the publisher (the “control group”), while other other users were shown the new version (the “treatment group”). 

![Yammer Publisher Example](/images/cases/yammer-publisher.png)

###The Problem

On July 1, you check the results of the A/B test. You notice that message posting is 50% higher in the treatment group&mdash;a hunge increase in posting. The table below summarizes the results:

<a href="https://modeanalytics.com/benn/reports/4194f44b1866/runs/dfb63bac58ab/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

The chart shows the average number of messages posted per user by treatment group. The table below provides additional test result details:

* **users:** The total number of users shown that version of the publisher.
* **total\_treated\_users**: The number of users who were treated in either group.
* **treatment_percent**: The number of users in that group as a percentage of the total number of treated users.
* **total:** The total number of messages posted by that treatment group.
* **average:** The average number of messages per user in that treatment group (total/users).
* **rate_difference:** The difference in posting rates between treatment groups (group average - control group average).
* **rate_lift:** The percent difference in posting rates between treatment groups ((group average / control group average) - 1).
* **stdev:** The standard deviation of messages posted per user for users in the treatment group. For example, if there were three people in the control group and they posted 1, 4, and 8 messages, this value would be the standard deviation of 1, 4, and 8 (which is 2.9).
* **t_stat:** A [test statistic](http://en.wikipedia.org/wiki/Student's_t-test) for calculating if average of the treatment group is statistically different from the average of the control group. It is calculated using the averages and standard deviations of the treatment and control groups.
* **p_value:** Used to determine the test's statistical significance.

The test above, which compares average posting rates between groups, uses a simple [Student's t-test](http://en.wikipedia.org/wiki/Student's_t-test) for deterimining statistical signficance. For testing on averages, t-tests are common, though other, more advanced statistical techniques are sometimes used. Furthermore, the test above uses a two-tailed test because the treatment group could perform either better or worse than the control group. ([Some argue](https://help.optimizely.com/hc/en-us/articles/200133789-How-long-to-run-a-test#calculating_significance) that one-tailed tests are better, however.) You can read more about the differences between one- and two-tailed t-tests [here](http://www.ats.ucla.edu/stat/mult_pkg/faq/general/tail_tests.htm).

Once you're comfortable with A/B testing, your job is to determine whether this feature is the real deal or too good to be true. The product team is looking to you for advice about this test, and you should try to provide as much information about what happened as you can.

###Getting Oriented

Before doing anything with the data, develop some hypotheses about why the result might look the way it does, as well as methods for testing those hypotheses. As a point of reference, such dramatic changes in user behavior&mash;like the 50% increase in posting&mdash;are extremely uncommon.

If you want to check your list of possible causes against ours, read the [first part of the answer key](answers/best-ab-test-ever-answers.html).

###The Data

For this problem, you will need to use four tables. *Note: This data is fake and was generated for the purpose of this case study. It is similar in structure to Yammer's actual data, but for privacy and security reasons, it is not real.*

<div class="accordion">
  <ul>
    {% include case-data/yammer-users.html %}
    {% include case-data/yammer-events.html %}
    {% include case-data/yammer-experiments.html %}
    {% include case-data/normal-distribution.html %}
  </ul>
</div>

###Validating the Results

Work through your list of hypotheses to determine whether the test results are valid. We suggest following the steps (and answer the questions) below:

* Check to make sure that this test was run correctly. Is the query that calculates lift and p-value correct? It may be helpful to start with the code that produces the above query, which you can find by clicking the link in the footer of the chart and navigating to the "query" tab.
* Check other metrics to make sure that this outsized result is not isolated to this one metric. What other metrics are important? Do they show similar improvements? This will require writing additional SQL queries to test other metrics.
* Check that the data is correct. Are there problems with the way the test results were recorded or the way users were treated into test and control groups? If something is incorrect, determine the steps necessary to correct the problem.
* Make a final recommendation based on your conclusions. Should the new publisher be rolled out to everyone? Should it be re-tested? If so, what should be different? Should it be abandoned entirely?

###Answers

If you want to check your work against the answer key, [click here](answers/best-ab-test-ever-answers.html#solution).