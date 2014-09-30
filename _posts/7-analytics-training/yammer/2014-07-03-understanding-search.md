---
layout: sqlschool-lesson
categories: analytics-training yammer
title:  "Understanding Search"
date:   2014-07-01 00:00:57
seo-title: 
description: 
---

*Before starting, be sure to read [the overview](/analytics-training/yammer/yammer.html) to learn a bit about Yammer as a company.*

The product team is determining priorities for the next development cycle and they are considering improving the site's search functionality. It currently works as follows:

* There is a search box in the header the persists on every page of the website. It prompts users to search for people, groups, and conversations.

<a href="/images/cases/yammer-search.png" class="with-caption image-link" title="Yammer's search bar">
  <img src="/images/cases/yammer-search.png" />  
</a>

* When a user begins to type in the search box, a dropdown list with the most relevant results appears. The results are separated by category (people, conversations, files, etc.). There is also an option to view all results.

<a href="/images/cases/yammer-search-autocomplete.png" class="with-caption image-link" title="Yammer's search auto-complete">
  <img src="/images/cases/yammer-search-autocomplete.png" />  
</a>

* When the user hits enter or selects “view all results” from the dropdown, she is taken to a results page, with results separated by tabs for different categories (people, conversations, etc.). Each tab is order by relevance and chronology (more recent posts surface higher).

<a href="/images/cases/yammer-search-results.png" class="with-caption image-link" title="Yammer's detailed search results">
  <img src="/images/cases/yammer-search-results.png" />  
</a>

* The search results page also has an “advanced search” box that allows the user to search again within a specific Yammer group or date range.

###The Problem

Before tackling search, the product team wants to make sure that the engineering team's time will be well-spent in doing so. After all, each new feature comes at the expense of some other potential feature(s). The product team is most interested in determining whether they should even work on search in the first place and, if so, how they should modify it.

###Getting Oriented

Before looking at the data, develop some hypotheses about how users might interact with search. What is the purpose of search? How would you know if it is fulfilling that purpose? How might you (quantitatively) understand the general quality of an individual user's search experience?

If you're looking for some hints, check out the [first part of the answer key](answers/understanding-search-answers.html).

###The Data
There are two tables that are relevant to this problem. Most critically, there are certain events that you will want to look into in the events table below:

* search\_autocomplete: This is logged when a user clicks on a search option from autocomplete
* search\_run: This is logged when a user runs a search and sees the search results page.
* search\_click\_X: This is logged when a user clicks on a search result. X, which ranges from 1 to 10, describes which search result was clicked.

The tables names and column definitions are listed below&mdash;click a table name to view information about that table.

<div class="accordion">
  <ul>
    {% include case-data/yammer-users.html %}
    {% include case-data/yammer-events.html %}
  </ul>
</div>

###Making a Recommendation

Once you have an understanding of the data, try to validate some of the hypotheses you formed earlier. In particular, you should seek to answer the following questions:

* Are users' search experiences generally good or bad? 
* Is search worth working on at all?
* If search is worth working on, what, specifically, should be improved?

Come up with a brief presentation describing the state of search at Yammer. Display your findings graphically. You should be prepared to recommend what, if anything, should be done to improve search. If you determine that you do not have sufficient information to test anything you deem relevant, discuss the caveats.

Finally, determine a way to understand whether your feature recommendations are actually improvements over the old search (assuming that anything you recommend will be completed).

###Answers
If you want to check your work against the answer key, [click here](answers/understanding-search-answers.html#solution).

