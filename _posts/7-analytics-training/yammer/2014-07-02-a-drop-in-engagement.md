---
layout: sqlschool-lesson
categories: analytics-training yammer
title:  "A Drop In Engagement"
date:   2014-07-01 00:00:58
seo-title: 
description: 
---

*Before starting, be sure to read [the overview](/analytics-training/yammer/yammer.html) to learn a bit about Yammer as a company.*

Yammer's Analysts are responsible for triaging product and business problems as they come up. In many cases, these problems surface through key metric dashboards that execs and managers check daily.

###The Problem

You show up to work Tuesday morning, September 2, 2014. The head of the Product team walks over to your desk and asks you what you think about the latest activity on the user engagement dashboards. You fire them up, and something immediately jumps out:

<a href="https://modeanalytics.com/tutorial/reports/f7aeca4599b7/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

The above chart shows engagement on a rolling 7-day basis. Yammer defines engagement as having made some type of server call by interacting with the product (shown in the data as events of type "engagement"). Any point in this chart can be interpreted as "the number of users who logged at least one engagement event over the 7 days prior."

You will notice that the point on August 1, 2014 shows 1,476 engaged users — that's the total number of people who engaged between midnight July 25 and midnight August 1.

You are responsible for determining what caused the dip shown above and, if appropriate, recommending solutions for the problem.

###Getting Oriented
Before you even touch the data, come up with a list of possible causes for the dip in retention shown in the chart above. Make a list and determine the order in which you will check them. Make sure to note how you will test each hypothesis. Think carefully about the criteria you use to order them and write down the criteria as well.

Also, make sure you understand what the above chart shows and does not show.

If you want to check your list of possible causes against ours, read the [first part of the answer key](answers/a-drop-in-engagement-answers.html).

###Digging In

Once you have an ordered list of possible problems, it's time to investigate.

For this problem, you will need to use four tables. The tables names and column definitions are listed below&mdash;click a table name to view information about that table. *Note: this data is fake and was generated for the purpose of this case study. It is similar in structure to Yammer's actual data, but for privacy and security reasons it is not real.*

<div class="accordion">
  <ul>
    {% include case-data/yammer-users.html %}
    {% include case-data/yammer-events.html %}
    {% include case-data/yammer-emails.html %}
    {% include case-data/dimension-rollup-periods.html %}
  </ul>
</div>

###Making a Recommendation

Start to work your way through your list of hypotheses in order to determine the source of the drop in engagement. As you explore, make sure to save your work. It may be helpful to start with the code that produces the above query, which you can find by clicking the link in the footer of the chart and navigating to the "query" tab.

Answer the following questions:

* Do the answers to any of your original hypotheses lead you to further questions?
* If so, what are they and how will you test them?
* If they are questions that you can't answer using data alone, how would you go about answering them (hypothetically, assuming you actually worked at this company)?
* What seems like the most likely cause of the engagement dip?
* What, if anything, should the company do in response?

###Answers

If you want to check your work against the answer key, [click here](answers/a-drop-in-engagement-answers.html#solution).