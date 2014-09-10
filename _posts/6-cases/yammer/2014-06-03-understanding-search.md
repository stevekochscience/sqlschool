---
layout: sqlschool-case
categories: cases yammer
title:  "Understanding Search"
date:   2014-06-01 00:00:57
seo-title: 
description: 
---

The product team is determining priorities for the next development cycle and they are considering improving the site's search functionality. It currently works as follows:

* There is a search box in the header the persists on every page of the website. It prompts users to search for people, groups, and conversations. <image>
* When a user begins to type in the search box, a dropdown list with the most relevant results appears. The results are separated by category (people, conversations, files, etc.). There is also an option to view all results.
* When the user hits enter or selects “view all results” from the dropdown, she is taken to a results page, with results separated by tabs for different categories (people, conversations, etc.). Each tab is order by relevance and chronology (more recent posts surface higher).
* The search results page also has an “advanced search” box that allows the user to search again within a specific Yammer group or date range.

###The Problem

Before tackling search, the product team wants to make sure that the engineering team's time will be well-spent in doing so. After all, each new feature comes at the expense of some other potential feature(s). The product team is most interested in determining whether they should even work on search in the first place and, if so, how they should modify it.

###The Solution

1. Before looking at the data, develop some hypotheses about how users might interact with search. What is the purpose of search? How would you know if it is fulfilling that purpose? How might you (quantitatively) understand the general quality of an individual user's search experience?

2. Formulate a series of tests that will help you determine the following:

* Are users' search experiences generally good or bad? 
* Is search worth working on at all?
* If search is worth working on, what, specifically, should be improved?

2b. If you determine that you do not have sufficient information to test any of the above, discuss the caveats.

3. Get started digging into the data. Complete your tests from step 2 and show the results graphically.

4. If search is improved, how will you determine whether the improvement was good?

###The points:

* Autocomplete is used regularly, in 25% of sessions. Search is only used in about 10% of sessions.
* Users typically only use autocomplete once or twice per session, suggesting it's probably performing ok.
* Users typically run search more than once per session, suggesting search is not delivering good results.
* Furthermore, many searches do not have runs, and there tend to be fewer clicks per session than searches.
* People click on all ten results frequently, and only the first result slightly more often than others, suggesting the ordering is not good either.
* Many users do not return to searching in the month following their first search, while many users do return to autocomplete searches.

This all suggests that autocomplete is performing ok, while search runs are not. 


https://modeanalytics.com/modeanalytics/lists/2abc9a78b465/runs/23905dfc4e89