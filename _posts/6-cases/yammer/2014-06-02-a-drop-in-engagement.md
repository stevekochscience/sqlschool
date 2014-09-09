---
layout: sqlschool-case
categories: cases yammer
title:  "A Drop In Engagement"
date:   2014-06-01 00:00:58
seo-title: 
description: 
---

Yammer's Analysts are responsible for triaging product and business problems as they come up. In many cases, these problems surface through key metric dashboards that execs and managers check daily.

###The Problem

You show up to work Tuesday morning, September 2, 2014. The head of the Product team walks over to your desk and asks you what you think about the latest activity on the user engagement dashboards. You fire them up, and something immediately jumps out:

<a href="https://staging.modeanalytics.com/tutorial/reports/f7aeca4599b7/embed" class="mode-embed">Mode Analysis</a><script src="https://staging.modeanalytics.com/embed/embed.js"></script>

The above chart shows engagement on a rolling 7-day basis. Yammer defines engagement as having made some type of server call by interacting with the product (shown in the data as events of type "engagement"). Any point in this chart can be interpreted as "the number of users who logged at least one engagement event over the 7 days prior."

You will notice that the point on August 1, 2014 shows 1,241 engaged users — that's the total number of people who engaged between XX and XX.

You are responsible for determining what caused the dip shown above and, if appropriate, recommending solutions for the problem.

###Getting Oriented
Before you even touch the data, come up with a list of possible causes for the dip in retention shown in the chart above. Make a list and determine the order in which you will check them. Make sure to note how you will test each hypothesis. Think carefully about the criteria you use to order them and write down the criteria as well.

Also, make sure you understand what the above chart shows and does not show.

###Digging In

Once you have an ordered list of possible problems, it's time to investigate.

For this problem, you will need to use three tables.

The first table lists Yammer users. This table includes one row per user. Each row has six columns:

* user\_id: A unique ID per user. Can be joined to user\_id in either of the other tables.
* created_at: The time the user was created (first signed up).
* state: The state of the user (active or pending).
* activated_at: The time the user was activated, if they are active.
* company_id: The ID of the user's company.
* language: The chosen language of the user.

The users table can be found here: [https://modeanalytics.com/tutorial/tables/yammer_users](https://modeanalytics.com/tutorial/tables/yammer_users)

The second table is an events table. This table includes one row per event, where an event is an action that a user has taken on Yammer. These events include login events, messaging events, search events, events logged as users progress through a signup funnel, events around received emails. Each row of the table has four columns:

* user\_id: The ID of the user logging the event. Can be joined to user\_id in either of the other tables.
* occurred_at: The time the event occurred.
* event_type: The general event type. These include signup events, email events, and engagement events. Yammer considers any user who logs an engagement event to be engaged. **MORE HERE ON DIFFERENT EVENT TYPES AND NAMES**
* event_name: The specific action the user took. This includes the step of the signup flow, the action taken around the email, or action taken in the product.
* location: The country from which the event was logged (collected through IP address).
* device: The type of device used to log the event.

The table can be found here: [https://modeanalytics.com/tutorial/tables/yammer_events](https://modeanalytics.com/tutorial/tables/yammer_events)

The third table shows events specific to email. It contains three columns:

* user\_id: The ID of the user to whom the event relates. Can be joined to user\_id in either of the other tables.
* occurred_at: The time the event occurred.
* action: The name of the event that occurred. "sent\_weekly\_digest" means that the user was delivered a digest email showing relevant conversations from the previous day. "email\_open" means that the user opened the email. "email\_clickthrough" means that the user clicked a link in the email.

The table can be found here: [https://modeanalytics.com/tutorial/tables/yammer_emails](https://modeanalytics.com/tutorial/tables/yammer_emails)

###Making a Recommendation

Start to work your way through your list of hypotheses in order to determine the source of the drop in engagement. As you explore, make sure to save your work. It may be helpful to start with the code that produces the above query, which you can find by clicking the link in the footer of the chart and navigating to the "query" tab.

Do the answers to any of your original hypotheses lead you to further questions? If so, what are they and how will you test them? If they are questions that you can't answer using data alone, how would you go about answering them (hypothetically, assuming you actually worked at this company)?

What seems like the most likely cause of the engagement dip? What, if anything, should the company do in response?

###Answers

-developing the list and criteria

-going through the list

If you want to quickly get a sense of what's happening across a number of metrics, you can use this list [Diagnostic](https://modeanalytics.com/modeanalytics/lists/068fecfaa655/runs/799fccf30b13)
