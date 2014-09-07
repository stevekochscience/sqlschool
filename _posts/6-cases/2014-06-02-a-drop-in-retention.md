---
layout: sqlschool-case
categories: cases yammer
title:  "Yammer Case Study: A Drop In Retention"
date:   2014-06-01 00:00:58
seo-title: 
description: 
---

Yammer's Analysts are responsible for triaging product and business problems as they come up. In many cases, these problems surface through key metric dashboards that execs and managers check daily.

###The Problem

You show up to work Monday morning, August 17, 2014. The head of the Product team walks over to your desk and asks you what you think about the latest activity on the user retention dashboards. You fire them up, and something immediately jumps out:

<a href="https://modeanalytics.com/benn/reports/f34ca3417b4e/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

The above chart shows retention on a rolling 28-day basis. Yammer defines a retained user as one who is engaged between 7 and 14 days after they activated. The graph below shows the retention rate - the percent of new users who retained.

You will notice that the point on August 1, 2014 shows **XX** engaged users — that's the total number of people who engaged between February 1 and February 28. It may be helpful to see the code that produces the above query, which you can find by clicking the link in the footer of the chart.

You are responsible for determining what caused the dip shown above and, if appropriate, recommending solutions for the problem.

###Getting Oriented
Before you even touch the data, come up with a list of possible causes for the dip in retention shown in the chart above. Make a list and determine the order in which you will check them. Make sure to note how you will test each hypothesis. Think carefully about the criteria you use to order them and write down the criteria as well.

Also, make sure you understand what the above chart shows and does not show.

###Digging In

Once you have an ordered list of possible problems, it's time to investigate. For this problem, you will need to use two tables.

The first is a users table. This table includes one row per user. Each row has five columns:

* user_id: A unique ID per user
* created_at: The time the user was created
* state: The state of the user (active or pending)
* activated_at: The time the user was activated, if they are active
* company_id: The ID of the user's company

The users table can be found here: [https://modeanalytics.com/benn/tables/user_tests](https://modeanalytics.com/benn/tables/user_tests)

The second table is an events table. This table includes one row per event, where an event is an action that a user has taken on Yammer. These events include login events, messaging events, search events, events logged as users progress through a signup funnel, events around received emails. Each row of the table has four columns:

* user_id: The ID of the user logging the event
* occurred_at: The time the event occurred
* event_type: The general event type. These include signup events, email events, and engagement events. Yammer considers any user who logs an engagement event to be engaged.
* event_name: The specific action the user took. This includes the step of the signup flow, the action taken around the email, or action taken in the product. 

The table can be found here: [https://modeanalytics.com/benn/tables/event_tests](https://modeanalytics.com/benn/tables/event_tests)

Start to work your way through your list of hypotheses in order to determine the source of the drop in retention. As you explore, make sure to save your work.

###Making a Recommendation

2b. Do the answers to any of your original hypotheses lead you to further questions? If so, what are they and how will you test them? If they are questions that you can't answer using data alone, how would you go about answering them (hypothetically, assuming you actually worked at this company)?

3. What seems like the most likely cause of the engagement dip? What, if anything, should the company do in response?


