---
layout: sqlschool-lesson
category: "The Mode SQL Quiz"
title:  "THE SQL QUIZ"
date:   2014-04-01 00:00:57
---

We heard that a lot of people have been using [SQL School](/) to prep for Analytics interviews. We don't believe in teaching to the test &mdash; you'll find a lot of information in SQL School that will be helpful on the job but likely won't come up in an onsite interview. That said, some test prep is helpful, too. This quiz includes questions similar to those that we at Mode have personally heard in interviews or that SQL School students have relayed to us.

You probably shouldn't even bother with this quiz until you've completed SQL School's [basic tutorial](/the-basics/basic-concepts.html) and most of the [intermediate tutorial](/intermediate/aggregation-functions.html). Some questions also draw from the lessons on [Subqueries](/advanced/subqueries.html) and [Window Functions](/advanced/window-functions.html).

###Part 1: Conecpts
These questions are meant to assess your understanding of how SQL manipulates data. You don't need to actually write any code &mdash; just write clear answers.

Explain the difference between an inner join and an outer join.

Let's say you have to tables: one with *n* rows and one with *m* rows. What are the minimum and maximum numbers of rows that could be generated as results by joining the two of them (one join of any type)?

###Part 2: Technical Questions

###Quiz data
The next set of questions will require you to actually write some SQL. The data for these problems are... ok, they're fake. They're meant to simulate the data you might find at a company whose product is a web application.

The first table is a list of all the users of that web application. Each user has a unqiue `user_id`, though many users may be associated with any given `company_id`. In the `state` column, "pending" means that the person has been invited to the app but has not actually completed the signup process, while "active" means that the user has completed the signup process. Start by taking a look:

    SELECT * FROM benn.fake_dimension_users

The second table is a log of actions performed by users of the service (typically referred to as an event log). Each row is a new event, and individual users can log an unlimited number of events. Most of the events are named sensibly &mdash; "login" indicates that the user logged into the web app at whatever time is indicated in the `occurred_at` column. `event_type` is pretty straightforward as well, except for "test_treatment," which you can ignore because it isn't relevant to any of these problems.

    SELECT * FROM benn.fake_fact_events

###Quiz time
Most of these problems can be solved in multiple ways. Try to solve each question as simply as possible.

Write a query that counts the number of unique users who were placed into control groups (`event_name` includes the word "control"). [Answer here](https://modeanalytics.com/tutorial/reports/3ed740b2bd82).

Write a query that shows the number of unique users who logged events each day. [Answer here](https://modeanalytics.com/tutorial/reports/2bb5d97f8a15).

Using your answer to the previous question, write a query that shows how many days fall into each of these groups: 0-99 unique users, 100-199 unique users, 200+ unique users. For bonus points, make a chart that shows the results. [Answer here](https://modeanalytics.com/tutorial/reports/f21a6ca54773).

Using your answer to the previous two questions, calculate the median number of unique users on a given day. [Answer here](https://modeanalytics.com/tutorial/reports/ff1d86584f93).

Write a query that shows the number of unique users who logged a "complete_signup" event at each company (make sure not to double-count users who log that event twice). [Answer here](https://modeanalytics.com/tutorial/reports/ab259c3265a6) and [alternative here](https://modeanalytics.com/tutorial/reports/ffef371920eb).

Write a query that counts the number of users who completed signup (logged "complete_signup" event) within 24 hours of creating their `user_id` (logging "create_user" event). [Answer here](https://modeanalytics.com/tutorial/reports/721b6f479880).

<!--
Calculate how long each active user has been active. [Answer here]().
-->

<!--
something about unions?
-->


###Help us help you
Since we at Mode aren't out there interviewing for jobs, much of the material in this quiz came from SQL School students. We'd really appreciate your help in continuing to develop this quiz so that future students can get the best possible experience. If you have suggestions on how to improve the existing questions or you come up with a question that you think we should add, please email us at [sqlschool@modeanalytics.com](mailto:sqlschool@modeanalytics.com).

If you found this quiz helpful, please <a href="https://twitter.com/share?url=http://sqlschool.modeanalytics.com&lang=en&text=Learn SQL with @ModeAnalytics plain-English tutorial. It's free!&conturl=http://sqlschool.modeanalytics.com&count=vertical" target="_blank">share it on Twitter</a>.

<!--
Need to study more?
-->