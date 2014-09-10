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

You will notice that the point on August 1, 2014 shows 1,368 engaged users — that's the total number of people who engaged between midnight July 25 and midnight August 1.

You are responsible for determining what caused the dip shown above and, if appropriate, recommending solutions for the problem.

###Getting Oriented
Before you even touch the data, come up with a list of possible causes for the dip in retention shown in the chart above. Make a list and determine the order in which you will check them. Make sure to note how you will test each hypothesis. Think carefully about the criteria you use to order them and write down the criteria as well.

Also, make sure you understand what the above chart shows and does not show.

If you want to check your list of possible causes against ours, read the [first part of the answer key](answers/a-drop-in-engagement-answers.html).

###Digging In

Once you have an ordered list of possible problems, it's time to investigate.

For this problem, you will need to use four tables. *Note: this data is fake and was generated for the purpose of this case study. It is similar in structure to Yammer's actual data, but for privacy and security reasons it is not real.*

<div class="accordion">
  <ul>
    <li>
      <input type="checkbox" checked>
      <i></i>
      <h4>Table 1: Users</h4>
      <table>
        <tr>
          <th colspan="2">This table includes one row per user, with descriptive information about that user's account.
            <!--<a href="https://modeanalytics.com/tutorial/tables/yammer_users">View table in Mode</a>-->
          </th>
        </tr>
        <tr>
          <td>user_id:</td>
          <td class="right">A unique ID per user. Can be joined to user_id in either of the other tables.</td>
        </tr>
        <tr>
          <td>created_at:</td>
          <td class="right">The time the user was created (first signed up)</td>
        </tr>
        <tr>
          <td>state:</td>
          <td class="right">The state of the user (active or pending)</td>
        </tr>
        <tr><td>activated_at:</td>
          <td class="right">The time the user was activated, if they are active</td></tr>
        <tr>
          <td>company_id:</td><td class="right">The ID of the user's company</td></tr>
        <tr>
          <td>language:</td><td class="right">The chosen language of the user</td></tr>
      </table>
    </li>
    <li>
      <input type="checkbox" checked>
      <i></i>
      <h4>Table 2: Events</h4>
      <table>
        <tr>
          <th colspan="2">This table includes one row per event, where an event is an action that a user has taken on Yammer. These events include login events, messaging events, search events, events logged as users progress through a signup funnel, events around received emails.
            <!--<a href="https://modeanalytics.com/tutorial/tables/yammer_events">View this table in Mode</a>-->
          </th>
        </tr>
        <tr>
          <td>user_id:</td>
          <td class="right">The ID of the user logging the event. Can be joined to user\_id in either of the other tables.</td>
        </tr>
        <tr>
          <td>occurred_at:</td>
          <td class="right">The time the event occurred.</td>
        </tr>
        <tr>
          <td>event_type:</td>
          <td class="right">The general event type. These include signup events, email events, and engagement events. Yammer considers any user who logs an engagement event to be engaged.
            <!--**MORE HERE ON DIFFERENT EVENT TYPES AND NAMES**-->
          </td>
        </tr>
        <tr><td>event_name:</td>
          <td class="right">The specific action the user took. This includes the step of the signup flow, the action taken around the email, or action taken in the product.</td></tr>
        <tr>
          <td>location:</td><td class="right">The country from which the event was logged (collected through IP address).</td></tr>
        <tr>
          <td>device:</td><td class="right">The type of device used to log the event.</td></tr>
      </table>
    </li>
    <li>
      <input type="checkbox" checked>
      <i></i>
      <h4>Table 3: Email Events</h4>
      <table>
        <tr>
          <th colspan="2">This table contains events specific to the sending of emails. It is similar in structure to the events table above.
            <!--<a href="https://modeanalytics.com/tutorial/tables/yammer_emails">View this table in Mode</a>-->
          </th>
        </tr>
        <tr>
          <td>user_id:</td>
          <td class="right">The ID of the user to whom the event relates. Can be joined to user\_id in either of the other tables.</td>
        </tr>
        <tr>
          <td>occurred_at:</td>
          <td class="right">The time the event occurred.</td>
        </tr>
        <tr>
          <td>action:</td>
          <td class="right">The name of the event that occurred. "sent\_weekly\_digest" means that the user was delivered a digest email showing relevant conversations from the previous day. "email\_open" means that the user opened the email. "email\_clickthrough" means that the user clicked a link in the email.
            <!--**MORE HERE ON DIFFERENT EVENT TYPES AND NAMES**-->
          </td>
        </tr>
      </table>
    </li>
    <li>
      <input type="checkbox" checked>
      <i></i>
      <h4>Table 4: Rollup Periods</h4>
      <table>
        <tr>
          <th colspan="2">The final table is a lookup table that is used to create rolling time periods. Though you could use the INTERVAL() function, creating rolling time periods is often easiest with a table like this. You won't necessarily need to use this table in queries that you write, but the column descriptions are provided here so that you can understand the query that creates the chart shown above.
            <!--<a href="https://modeanalytics.com/tutorial/tables/dimension_rollup_periods">View this table in Mode</a>-->
          </th>
        </tr>
        <tr>
          <td>period_id:</td>
          <td class="right">This identifies the type of rollup period. The above dashboard uses period 1007, which is rolling 7-day periods.</td>
        </tr>
        <tr>
          <td>time_id:</td>
          <td class="right">This is the identifier for any given data point &mdash; it's what you would put on a chart axis. If time\_id is 2014-08-01, that means that is represents the rolling 7-day period leading up to 2014-08-01.</td>
        </tr>
        <tr>
          <td>pst_start:</td>
          <td class="right">The start time of the period in PST. For 2014-08-01, you'll notice that this is 2014-07-25 &mdash; one week prior. Use this to join events to the table.</td>
        </tr>
        <tr>
          <td>pst_end:</td>
          <td class="right">The start time of the period in PST. For 2014-08-01, the end time is 2014-08-01. You can see how this is used in conjunction with pst_start to join events to this table in the <a href="https://staging.modeanalytics.com/tutorial/reports/f7aeca4599b7/runs/5e08a9650c91/query">query that produces the above chart</a>.</td>
        </tr>
        <tr>
          <td>utc_start:</td>
          <td class="right">The same as pst_start, but in UTC time.</td>
        </tr>
        <tr>
          <td>pst_start:</td>
          <td class="right">The same as pst_end, but in UTC time.</td>
        </tr>
      </table>
    </li>
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