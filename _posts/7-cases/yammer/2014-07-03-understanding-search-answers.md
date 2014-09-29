---
layout: sqlschool-lesson
categories: cases yammer answers
title:  "Understanding Search: Answers"
date:   2014-07-01 00:00:57
seo-title: 
description: 
---
###Developing Hypotheses
Framing problems simply and correctly can often save time later on. Thinking about the ultimate purpose of search right off the bat can make it easier to evaluate other parts of them problem. Search, at the most basic level, is about helping people find what they're looking for easily. A great search product achieves this quickly and with minimal work on behalf of the user.

To understand whether search is fulfilling that purpose, consider some possibilities:

* **Search use:** The first thing to understand is whether anyone even uses search at all
* **Search frequency:** If users search a lot, it's likely that they're getting value out of the feature &dash; with a major exception. If users search repeatedly within a short timeframe, it's likely that they're refining their terms because they were unable to find what they wanted initially.
* **Repeated terms:** A better way to understand the above would be to actually compare similarity of search terms. That's much slower and more difficult to actually do than counting the number of searches a user performs in a short timeframe, so best to ignore this option.
* **Clickthroughs:** If a user clicks many links in the search results, it's likely that she isn't having a great experience. However, the inverse is not necessarily true &mdash; clicking only one result does *not* imply a success. If the user clicks through one result, then refines her search, that's certainly not a great experience, so search frequency is probably a better way to understand that piece of the puzzle. Clickthroughs are, however, very useful in determining whether search rankings are good. If users frequently click low results or scroll to additional pages, then the ranking algorithm should probably be adjusted.
  
* **Autocomplete Clickthroughs:** The autocomplete feature is certainly part of the equation, though its success should be measured separately to understand its role.

<div id="solution"></div>
###The State of Search
The criteria above suggest that understanding search on a session by session basis is going to be important for this problem. So before seeking to understand whether search is good or bad, it would be wise to define a session for the purposes of this problem, both practically and in terms of the data. For the following solution, a session is defined as a string of events logged by a user without a 10-minute break between any two events. So if a user goes 10 minutes without logging an event, the session is ended and her next engagement will be considered a new session.

Each step is linked, below, but if you want to follow along with all of the charts in one view, [check them out here](https://modeanalytics.com/modeanalytics/lists/2abc9a78b465/runs/23905dfc4e89).

**1.** First, take a look at how often people search and whether that changes over time. Users take advantage of the autocomplete function more frequently than they actually run searches that take them to the search results page:

<a href="https://modeanalytics.com/benn/reports/c1039e799f77/runs/21a44c0d302d/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

To be more precise, autocomplete gets used in approximately 25% of sessions, while search is only used in 8% or so:
<a href="https://modeanalytics.com/benn/reports/c1039e799f77/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

Autocomplete's 25% use indicates that there is a need for users to find information on their Yammer networks. In other words, it's a feature that people use and is worth some attention.

**2.** As you can see below, autocomplete is typically used once or twice per session:

<a href="https://modeanalytics.com/benn/reports/83610ace1078/runs/10f5c4fa2c0d/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

When users do run full searches, they typically run multiple searches in a single session. Considering full search is a more rarely used feature, this suggests that either the search results are not very good or that there is a very small group of users who like search and use it all the time:

<a href="https://modeanalytics.com/benn/reports/5e26ef62a86a/runs/6961dbe427ea/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**3.** Digging in a bit deeper, it's clear that search isn't performing particularly well. In sessions during which users do search, they almost never click any of the results:

<a href="https://modeanalytics.com/benn/reports/6bee31ccfa53/runs/7854d26dd339/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

Furthermore, more searches in a given session do not lead to many more clicks, on average:

<a href="https://modeanalytics.com/benn/reports/0fd7ebdac16b/runs/bd0455fb03dc/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**4.** When users do click on search results, their clicks are fairly evenly distributed across the result order, suggesting the ordering is not very good. If search were performing well, this would be heavily weighted toward the top two or three results:

<a href="https://modeanalytics.com/benn/reports/12aeb5489382/runs/62a6c94929f2/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**5.** Finally, users who run full searches rarely do so again within the following month:

<a href="https://modeanalytics.com/benn/reports/f5a390533fd3/runs/e1d68b3df1ca/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

Users who use the autocomplete feature, by comparison, continue to use it at a higher rate:

<a href="https://modeanalytics.com/benn/reports/74e69cfb70fb/runs/8e510800a865/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

###Follow Through

This all suggests that autocomplete is performing reasonably well, while search runs are not. The most obvious place to focus is on the ordering of search results. It's important to consider that users likely run full searches *when autocomplete does not provide the things they are looking for*, so maybe changing the search ranking algorithm to provide results that are a bit different from the autocomplete results would help. Of course, there are many ways to approach the problem &mdash; the important thing is that the focus should be on improving full search results.
