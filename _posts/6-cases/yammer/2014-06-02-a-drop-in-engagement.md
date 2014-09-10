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

You will notice that the point on August 1, 2014 shows 1,241 engaged users — that's the total number of people who engaged between midnight July 25 and midnight August 1.

You are responsible for determining what caused the dip shown above and, if appropriate, recommending solutions for the problem.

###Getting Oriented
Before you even touch the data, come up with a list of possible causes for the dip in retention shown in the chart above. Make a list and determine the order in which you will check them. Make sure to note how you will test each hypothesis. Think carefully about the criteria you use to order them and write down the criteria as well.

Also, make sure you understand what the above chart shows and does not show.

###Digging In

Once you have an ordered list of possible problems, it's time to investigate.

For this problem, you will need to use four tables. *Note: this data is fake and was generated for the purpose of this case study. It is similar in structure to Yammer's actual data, but for privacy and security reasons it is not real.*


<style>
 .transition, p, table, ul li i:before, ul li i:after {
  transition: all 0.25s ease-in-out;
}

 .flipIn, h1, ul li {
  animation: flipdown 0.5s ease both;
}

 .no-select {
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

 .accordion h4 {
  display: block;
  margin: 0;
  padding-left: 0px;
  cursor: pointer;
}

 .accordion p {
  color: rgba(48, 69, 92, 0.8);
  font-size: 17px;
  line-height: 26px;
  letter-spacing: 1px;
  position: relative;
  overflow: hidden;
  max-height: 800px;
  opacity: 1;
  transform: translate(0, 0);
  margin-top: 14px;
  
}

.accordion table {
  position: relative;
  overflow: hidden;
  opacity: 1;
  transform: translate(0, 0);
  margin-top: 14px;
  max-height:800px;
}

 .accordion ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
 .accordion ul li {
  list-style: none;
  position: relative;
  padding: 0;
  margin: 0;
  padding-bottom: 4px;
  padding-top: 18px;
  border-top: 1px dotted #dce7eb;
}
 .accordion ul li:nth-of-type(1) {
  animation-delay: 0.5s;
}
 .accordion ul li:nth-of-type(2) {
  animation-delay: 0.75s;
}
 .accordion ul li:nth-of-type(3) {
  animation-delay: 1s;
}
 .accordion ul li:last-of-type {
  padding-bottom: 0;
}
 .accordion ul li i {
  position: absolute;
  transform: translate(-6px, 0);
  margin-top: 16px;
  right: 0;
}
 .accordion ul li i:before, ul li i:after {
  content: "";
  position: absolute;
  background-color: #ff6873;
  width: 3px;
  height: 9px;
}
 .accordion ul li i:before {
  transform: translate(-2px, 0) rotate(45deg);
}
 .accordion ul li i:after {
  transform: translate(2px, 0) rotate(-45deg);
}
 .accordion ul li input[type=checkbox] {
  position: absolute;
  cursor: pointer;
  width: 100%;
  height: 100%;
  z-index: 1;
  opacity: 0;
}
 .accordion ul li input[type=checkbox]:checked ~ p {
  margin-top: 0;
  max-height: 0;
  opacity: 0;
  transform: translate(0, 50%);
}
 .accordion ul li input[type=checkbox]:checked ~ table {
  margin-top: 0;
  max-height: 0;
  opacity: 0;
  display: none;
  transform: translate(0, 50%);
}
 .accordion ul li input[type=checkbox]:checked ~ i:before {
  transform: translate(2px, 0) rotate(45deg);
}
 .accordion ul li input[type=checkbox]:checked ~ i:after {
  transform: translate(-2px, 0) rotate(-45deg);
}

@keyframes flipdown {
  0% {
    opacity: 0;
    transform-origin: top center;
    transform: rotateX(-90deg);
  }
  5% {
    opacity: 1;
  }
  80% {
    transform: rotateX(8deg);
  }
  83% {
    transform: rotateX(6deg);
  }
  92% {
    transform: rotateX(-3deg);
  }
  100% {
    transform-origin: top center;
    transform: rotateX(0deg);
  }
}
</style>
<div class="accordion">
  <ul>
    <li>
      <input type="checkbox" checked>
      <i></i>
      <h4>Table 1: Users</h4>
      <table>
        <tr><td colspan="2">This table includes one row per user. Each row has six columns:</td></tr>
        <tr><td colspan="2"><a href="https://modeanalytics.com/tutorial/tables/yammer_users">View table in Mode</a></td></tr>
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
        <tr><td colspan="2">This table includes one row per event, where an event is an action that a user has taken on Yammer. These events include login events, messaging events, search events, events logged as users progress through a signup funnel, events around received emails. Each row of the table has four columns:</td></tr>
        <tr><td colspan="2"><a href="https://modeanalytics.com/tutorial/tables/yammer_events">View in Mode</a></td></tr>
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
          <td class="right">The general event type. These include signup events, email events, and engagement events. Yammer considers any user who logs an engagement event to be engaged. **MORE HERE ON DIFFERENT EVENT TYPES AND NAMES**</td>
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
        <tr><td colspan="2">This table contains three columns:</td></tr>
        <tr><td colspan="2"><a href="https://modeanalytics.com/tutorial/tables/yammer_emails">View in Mode</a></td></tr>
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
          <td class="right">The name of the event that occurred. "sent\_weekly\_digest" means that the user was delivered a digest email showing relevant conversations from the previous day. "email\_open" means that the user opened the email. "email\_clickthrough" means that the user clicked a link in the email. **MORE HERE ON DIFFERENT EVENT TYPES AND NAMES**</td>
        </tr>
      </table>
    </li>
    <li>
      <input type="checkbox" checked>
      <i></i>
      <h4>Table 4: Rollup Periods</h4>
      <table>
        <tr><td colspan="2">The final table is a lookup table that is used to create rolling time periods. Though you could use the `INTERVAL()` function, creating rolling time periods is often easiest with a table like this. You won't necessarily need to use this table in queries that you write, but the column descriptions are provided here so that you can understand the query that creates the chart shown above:</td></tr>
        <tr><td colspan="2"><a href="https://modeanalytics.com/tutorial/tables/dimension_rollup_periods">View in Mode</a></td></tr>
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

#Answers

###Preparation and Prioritizing

Making hypotheses and evaluating them is often the most important part of this problem. If you do this well, you can save yourself a lot of time spent digging through data. It's impossible to provide an exhaustive list of possibilities for this kind of problem, but here are some things we can up with in our brainstorming session:

* **Holiday:** It's likely that people using a work application like Yammer might engage at a lower rate on holidays. If one country has much lower engagement than others, it's possible that this is the cause.
* **Broken** feature: It is possible that something in the application is broken, and therefore impossible for people to use. This is a little harder to pinpoint because different parts of the application would show differently in the metrics. For example, if something in the signup flow broke, preventing new users from joining Yammer, growth would also be down. If a Mobile app was unstable and crashed, engagement would be down for only that device type.
* **Broken tracking code:** It's possible that the code that logs events is, itself, broken. If you see a drop to absolutely zero events of a certain type and you rule out a broken feature, then this is a possibility.
* **Traffic anomalies from bots:** Most major website see a lot of activity from bots. A change in the product or infrastructure that might make it harder for bots to interact with the site could decrease engagement (assuming bots look like real users). This is tricky to determine because you have to identify bot-like behavior through patterns or specific events.
* **Traffic shutdown to your site:** It is possible for internet service providers to block your site. This is pretty rare for professional applications, but nevertheless possible. 
* **Marketing event:** A Super Bowl ad, for example, might cause a massive spike in sign-ups for the product. But users who enter through one-time marketing blitzes often retain at lower rates than users who are referred by friends, for example. Because the chart uses a rolling 7-day period, this will register as high engagement for one week, then almost certainly look like a big drop in engagement the following week. Most often, the best way to determine this is to simply ask someone in the Marketing department if anything big happened recently.
* **Bad data:** There are lots of ways to log bad data. For example, most large web apps separate their QA data from production data. One way or another, QA data can make its way into the production database. This is not likely to be the problem in this particular case, as it would likely show up as additional data logged from very few users.
* **Search crawler changes:** For a website that receives a lot of traffic, changes in the way search engines index them could cause big swings in traffic.

That's a lot of possibilities, so it's important to move through them in the most efficient order possible. Here are some suggestions for how to sort them so that you don't waste time:

* **Experience:** This isn't particularly relevant for those of you who have not worked in industry before, but once you have seen these problems a couple time, you will get a sense for the most frequent problems.
* **Communication:** It's really easy to ask someone about marketing events, so there's very little reason not to do that. Unfortunately, this is also irrelevant for this example, but it's certainly worth mentioning.
* **Speed:** Certain scenarios are easier to test than others, sometimes because the data is cleaner or easier to understand or query, sometimes because you've done something similar in the past. If two possibilities seem equally likely, test the faster one first.
* **Dependency:** If a particular scenario will be easy to understand after testing a different scenario, then test them in the order that makes sense.

###Solving the Case

The answers to the first three questions depend heavily on the individual's approach. It would be impossible to list answers for all possible hypotheses, but here's an example of how a solid thought process might look, all the way from the beginning to the solution.

Each step is linked, below, but if you want to follow along with all of the charts in one view, [check them out here](https://modeanalytics.com/modeanalytics/lists/068fecfaa655/runs/799fccf30b13).

**1.**One of the easiest things to check is growth, both because it's easy to measure and because most companies (Yammer included) track this closely already. In this case, you have to make it yourself, though. You'll notice that nothing has really changed about the growth rate &mdash; it continues to be high during the week, low on weekends:

<a href="https://modeanalytics.com/benn/reports/bae848ef66b9/runs/b404fb0cb24e/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**2.**Since growth is normal, it's possible that the dip in engagement is coming from existing users. One of the most effective ways to look at this is to cohort users based on when they signed up for the product. This chart shows a decrease in engagement among users who signed up more than 10 weeks prior:

<a href="https://modeanalytics.com/benn/reports/26c4bbb533cf/runs/650252353750/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**3.**The understanding that the problem is localized to older users leads us to believe that the problem probably isn't related to a one-time spike from marketing traffic or something that is affecting new traffic to the site like being blocked or changing rank on search engines. Now let's take a look at various device types to see if the problem is localized to any particular product:

<a href="https://modeanalytics.com/benn/reports/dea5145896c6/runs/9102e3ca0728/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**4.**If you filter the above chart down to phones (double-click the dot next to "phone" in the legend), you will see that there's a pretty steep drop in phone engagement rates. So it's likely that there's a problem with the mobile app related to long-time user retention. At this point, you're in a good position to ask around and see if anything changed recently with the mobile app to try to figure out the problem. You might also think about what causes people to engage with the product. The purpose of the digest email mentioned above is to bring users back into the product. Since we know this problem relates to the retention of long-time users, it's worth checking out whether the email has something to do with it:

<a href="https://modeanalytics.com/benn/reports/6e5a7d29a5bd/runs/8df1fff51bc7/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

**5.**If you filter to clickthroughs (again, by clicking the dot in the legend), you'll see that clickthroughs are way down. This next chart shows in greater detail clickthrough and open rates of emails, indicating clearly that the problem has to do with digest emails in addition to mobile apps.

<a href="https://modeanalytics.com/benn/reports/f6b7e880b076/runs/964aeeabe0d1/embed" class="mode-embed">Mode Analysis</a><script src="https://modeanalytics.com/embed/embed.js"></script>

###Follow Through
After investigation, it appears that the problem has to do with mobile use and digest emails. The intended action here should be clear: notify the head of product (who approached you in the first place) that the problem is localized in these areas and that it's worth checking to make sure something isn't broken or poorly implemented. It's not clear from the data *exactly* what the problem is or how it should be solved, but the above work can save other teams a lot of time in figuring out where to look.