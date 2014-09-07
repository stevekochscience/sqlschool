---
layout: sqlschool-lesson
category: "intermediate"
title:  "Aggregating Within Categories"
date:   2014-02-01 00:00:58
seo-title: "GROUP BY and Aggregations"
description: "Learn to use the SQL GROUP BY clause with real-world examples. Free, interactive SQL tutorials to develop your data analysis skills."
---

This lesson uses the same data as the [previous lesson](/intermediate/aggregation-functions.html), [Apple](http://www.apple.com) stock price data. The data was pulled from [Google Finance](http://finance.google.com) in January 2014. There is one row for each day (indicated in the `date` field). `open` and `close` are the opening and closing prices of the stock on that day. `high` and `low` are the high and low prices for that day. `volume` is the number of shares traded on that day. Some data has been intentionally removed for the sake of this lesson.

    SELECT * FROM tutorial.aapl_historical_stock_price

<div id="group-by"></div>
###GROUP BY
<!-- Not sure which one of us would be best suited to tackling this.  This whole section needs to be completely revisited. -->
All of the queries in the [previous lesson](/intermediate/aggregation-functions.html) have something in common: they all aggregate across the entire table. What if you want to aggregate only part of a table &mdash; you want to count the number of entries for each year, for example. `GROUP BY` allows you to separate data into groups which can be aggregated independent of one another. Here's an example:

    SELECT year,
           COUNT(*) AS count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY year

You can group by multiple columns, but you have to separate column names with commas &mdash; just as with `ORDER BY`:

    SELECT year,
           month,
           COUNT(*) AS count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY year, month

<div class="practice-prob">
  Calculate the total number shares traded each month. Order your results chronologically.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/36b338ee7ec5" target="_blank">See the Answer &raquo;</a>
</div>

As in [ORDER BY](/the-basics/order-by.html), you can substitute numbers for column names in the `GROUP BY` clause. It's generally recommended to do this only when you are grouping many columns, or if something else is causing the text in the `GROUP BY` clause to be excessively long:

    SELECT year,
           month,
           COUNT(*) AS count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY 1, 2

*Note: this functionality (numbering columns instead of using names) is supported by Mode, but not by every flavor of SQL, so if you're using another system or connected to certain types of databases, it may not work.*

The order of column names in your `GROUP BY` clause doesn't matter &mdash; the results will be the same regardless. If you want to control how the aggregations are grouped together, use `ORDER BY`. Try running the query below, then reverse the column names in the `ORDER BY` statement and see how it looks:

    SELECT year,
           month,
           COUNT(*) AS count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY year, month
     ORDER BY month, year

There's one thing to be aware of as you group by multiple columns: SQL evaluates the aggregations before the `LIMIT` clause. If you don't group by any columns, you'll get a 1-row result &mdash; no problem there. If you group by a column with enough unique values that it exceeds the `LIMIT` number, what will happen is that aggregates will be calculated, and then some rows will simply be omitted from the results. This is actually a nice way to do things because you know you're going to get the correct aggregates. If SQL cut the table down to 100 rows, then performed the aggregations, your results would be substantially different. The above query's results exceed 100 rows, so it is a perfect example. Try removing the limit and running it again.

<div id="having"></div>
###HAVING
Now, let's say that it's not enough just to know aggregated stats by month. After all, there are a lot of months in this dataset. Instead, let's say that you want to find every month during which AAPL stock worked its way over $400/share. The `WHERE` clause won't work for this because it doesn't allow you to filter on aggregate columns &mdash; that's where the `HAVING` clause comes in:

    SELECT year,
           month,
           MAX(high) AS month_high
      FROM tutorial.aapl_historical_stock_price
     GROUP BY year, month
    HAVING MAX(high) > 400
     ORDER BY year, month

Note: `HAVING` is the "clean" way to filter a query that has been aggregated, but this is also commonly done using a subquery, which you will learn about in a [later lesson](/advanced/subqueries.html).

<!--
<div class="practice-prob">
  Calculate the total number shares traded each month. Order your results chronologically.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/36b338ee7ec5" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Calculate the total number shares traded each month. Order your results chronologically.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/36b338ee7ec5" target="_blank">See the Answer &raquo;</a>
</div>
-->

###Query Clause Order
As mentioned in prior tutorials, the order in which you write the clauses is important. Here's the order for everything you've learned so far:

1. `SELECT`
2. `FROM`
3. `WHERE`
4. `GROUP BY`
5. `HAVING`
6. `ORDER BY`

<!-- Maybe include something about where the things we are learning on this page fit in?  Otherwise this seems like an arbitrary location for this section.
-->

###Aggregation Practice

<div class="practice-prob">
  Write a query to calculate the average daily price change in Apple stock, grouped by year
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/c1881e6c354d" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that calculates the lowest and highest prices that Apple stock achieved each month.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/9d9f9dcd83bb" target="_blank">See the Answer &raquo;</a>
</div>