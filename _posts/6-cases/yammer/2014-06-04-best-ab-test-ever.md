---
layout: sqlschool-case
categories: cases yammer
title:  "The Best AB Test Ever"
date:   2014-06-01 00:00:56
seo-title: 
description: 
---

Yammer develops new features, but they also spend a lot of time improving the product's core functionality. This case focuses on an improvement to the “publisher” — the module into which users type their messages. The product team ran an AB test from June 1 through June 30, during which some Yammer users (“control group”) were shown the old version of the publisher every time they used the product. All other users (“treatment group”) were shown a different version.

**<images of the two publishers>**

###The Problem

On July 1, you check the results and notice that message posting is 50% higher in the treatment group. Here's how to read the table below:

* **users:** the total number of users given that version of the publisher.
* **treatment_percent**: the number of users in that group as a percentage of the total number of users.
* **total:** 

<a href="https://modeanalytics.com/benn/reports/4194f44b1866/runs/dfb63bac58ab/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

If you're a little confused by all of this, you might want to read a bit more about [the math behind AB testing](LINK).

Once you're comfortable with AB testing, your job will be to determine whether this feature is the real deal or too good to be true.

###Getting Oriented

Before doing anything with the data, develop some hypotheses about why the result might look the way it does, as well as methods for testing those hypotheses. As a point of reference, a single-digit percentage increase in message posting would be considered very strong, and 50% is basically unheard of.

If you want to check your list of possible causes against ours, read the [first part of the answer key](answers/best-ab-test-ever-answers.html).

###The Data

For this problem, you will need to use four tables. *Note: this data is fake and was generated for the purpose of this case study. It is similar in structure to Yammer's actual data, but for privacy and security reasons it is not real.*

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
* Check other metrics to make sure that this outsized result is not isolated to this one metric. What metrics are important? Do they show similar improvements? This will require writing additional SQL queries to test other metrics.
* Check that the data is correct. Are there problems with the way the test results were recorded or the way users were sorted into test and control groups? If something is incorrect, determine the steps necessary to correct the problem.
* Make a final recommendation based on your conclusions. Should the new publisher be rolled out to everyone? Should it be re-tested? If so, what should be different? Should it be abandoned entirely?

###Answers

If you want to check your work against the answer key, [click here](answers/best-ab-test-ever-answers.html#solution).