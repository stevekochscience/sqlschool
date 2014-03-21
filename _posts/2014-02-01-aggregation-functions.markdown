---
layout: sqlschool-lesson
category: "intermediate"
title:  "Summary Statistics: Using SQL to Replace Pivot Tables"
date:   2014-02-01 00:00:59
---

Welcome to the Intermediate SQL Tutorial! If you skipped [The Basics](/the-basics/basic-concepts.html), you should take a quick peek [at this page](/the-basics/select-from.html) to get an idea of how to get the most out of this tutorial. For convenience, here's the gist:

* Open another window to [Mode](http://modeanalytics.com). Make an account if you don't have one.
* For each lesson, start by running `SELECT *` on the relevant dataset so you get a sense of what the raw data looks like. Do this in that window you just opened to Mode.
* Run all of the code blocks in the lesson in Mode in the other window. You'll learn more if you really examine the results and understand what the code is doing.

In the previous tutorial, many of the practice problems could only be solved in one or two ways with the skills you had learned. As you learn more skills and problems get harder, there will be many ways of producing the correct results. Keep in mind that the answers to practice problems should be used as a reference, but are by no means the only ways of answering the questions.  

###Today's dataset

For this lesson and the next, you'll be working with [Apple](http://www.apple.com) stock price data. The data was pulled from [Google Finance](http://finance.google.com) in January 2014. There is one row for each day (indicated in the `date` field). `open` and `close` are the opening and closing prices of the stock on that day. `high` and `low` are the high and low prices for that day. `volume` is the number of shares traded on that day. Some data has been intentionally removed for the sake of this lesson. Check it out for yourself:

    SELECT * FROM tutorial.aapl_historical_stock_price

###Aggregation Functions
As the [beginner tutorial](/the-basics/basic-concepts.html) points out, SQL is excellent at aggregating data the way you might in a [pivot table](http://en.wikipedia.org/wiki/Pivot_table) in Excel. You will use aggregation functions all the time, so it's important to get comfortable with them. The functions themselves are the same ones you will find in Excel or any other analytics program: `COUNT`, `SUM`, `MIN`, `MAX`, `AVG`. The [beginner tutorial](/the-basics/where-operators.html) also pointed out that arithmetic operators only perform operations across rows. Aggregation functions are used to perform operations across entire columns (which could include millions of rows of data or more).

###COUNT
It's easiest to start with `COUNT` because verifying your results is extremely simple. Let's begin by using `*` to select all rows.


    SELECT COUNT(*)
      FROM tutorial.aapl_historical_stock_price

<em>Note: Typing</em> <code>COUNT(1)</code> <em>has the same effect as</em> <code>COUNT(\*)</code><em>.  Which one you use is a matter of personal preference.</em>

<!--
DEREK: think this was your note.

You can see that the result showed a count of all rows to be 3555. Turn the limit off and run the following query and note that Mode actually provides a row count, which should be the same as the result of the above query:

    SELECT * FROM tutorial.aapl_historical_stock_price

-->
Things start to get a little bit tricky when you want to count individual columns. The following code will provide a count of all of rows in which the `high` column **is not null**.

    SELECT COUNT(high)
      FROM tutorial.aapl_historical_stock_price

You'll notice that this result is lower than what you got with `COUNT(*)`. That's because `high` has some nulls. In this case, we've deleted some data to make the lesson interesting, but it might often be that case that you will run into naturally-occurring null rows. For example, imagine you've got a table with one column showing email addresses for everyone you sent a marketing email to, and another column showing the date and time that each person opened the email. If someone didn't open the email, the date/time field would likely be null.

<div class="practice-prob">
  Write a query to count the number of non-null rows in the <code>low</code> column.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/ce67f767fd35" target="_blank">See the Answer &raquo;</a>
</div>

One nice thing about `COUNT` is that you can use it on non-numerical columns:

    SELECT COUNT(date)
      FROM tutorial.aapl_historical_stock_price

The above query returns the same result as the previous: 3555 &mdash; It's hard to tell because each row has a different `date` value, but `COUNT` simply counts the total number of non-null rows, not the distinct values. Counting the number of distinct values in a column is discussed in a [later tutorial](/intermediate/distinct.html). 

You might have also noticed that the column header in the results just reads "count." We recommend naming your columns so that they make a little more sense to the reader. As mentioned in an [earlier lesson](/the-basics/select-from.html), it's best to use lower case letters and underscores. You can add column names (also called *aliases*) using `AS`:

    SELECT COUNT(date) AS count_of_date
      FROM tutorial.aapl_historical_stock_price

If you must use spaces, you will need to use double quotes.

    SELECT COUNT(date) AS "Count Of Date"
      FROM tutorial.aapl_historical_stock_price

*Note: This is really the only place in which you'll ever want to use double quotes in SQL; single quotes for everything else.*

<div class="practice-prob">
  Write a query that determines counts of every single column. Which column has the most null values?
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/4da53e30e228" target="_blank">See the Answer &raquo;</a>
</div>

###SUM
As its name suggests, `SUM` totals all the values in a given column.  Unlike `COUNT`, you can only use `SUM` on columns containing numerical values.

    SELECT SUM(volume)
      FROM tutorial.aapl_historical_stock_price

An important thing to remember: aggregators only aggregate vertically. If you want to perform a calculation across rows, you would do this with [simple arithmetic](/the-basics/where-operators.html).

You don't need to worry as much about the presence of nulls with `SUM` as you would with `COUNT`, as `SUM` treats nulls as 0.

<div class="practice-prob">
  Write a query to calculate the average opening price (hint: you will need to use both <code>COUNT</code> and <code>SUM</code>, as well as some simple arithmetic.).
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/4106c16551ac" target="_blank">See the Answer &raquo;</a>
</div>

###MIN/MAX
`MIN` and `MAX` are similar to `COUNT` in that they can be used on non-numerical columns. Depending on the column type, `MIN` will return the lowest number, earliest date, or non-numerical value as close to "a" as possible. As you might suspect, `MAX` does the opposite:

    SELECT MIN(volume) AS min_volume,
           MAX(volume) AS max_volume
      FROM tutorial.aapl_historical_stock_price

Nulls are treated as lower than 0 or "a" so `MIN` will return a null value if the column contains one. If you want the lowest real number or value, you can filter out nulls using a `WHERE` clause:

    SELECT MIN(volume) AS min_volume,
           MAX(volume) AS max_volume
      FROM tutorial.aapl_historical_stock_price
     WHERE volume IS NOT NULL

<div class="practice-prob">
  What was Apple's lowest stock price (at the time of this data collection)?
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/f374f60f4e9c" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  What was the highest single-day increase in Apple's share value?
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/1ed0029e2c68">See the Answer &raquo;</a>
</div>

###AVG
`AVG` does what you'd think &mdash; it calculates the average of selected values. It is very useful, but has some limitations. First, it can only be used on numerical columns. Second, it ignores nulls completely. You can see this by comparing these two queries:

    SELECT AVG(high)
      FROM tutorial.aapl_historical_stock_price
     WHERE volume IS NOT NULL

Produces the same result as:

    SELECT AVG(high)
      FROM tutorial.aapl_historical_stock_price

There are some cases in which you will want to treat null values as 0. For these cases, you'll want to write a statement that changes the nulls to 0 (covered in a [later lesson](/intermediate/case.html)).

<div class="practice-prob">
  Write a query that calculates the average daily trade volume for Apple stock.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/0328fbae6c07" target="_blank">See the Answer &raquo;</a>
</div>

###GROUP BY
<!-- Not sure which one of us would be best suited to tackling this.  This whole section needs to be completely revisited. -->
All of the queries above have something in common: they all aggregate across the entire table. What if you want to aggregate only part of the table &mdash; you want to count the number of entries for each year, for example. `GROUP BY` allows you to separate data into groups which can be aggregated independent of one another. Here's an example:

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

###Query Clause Order
As mentioned in prior tutorials, the order in which you write the clauses is important. Here's the order for everything you've learned so far:

1. `SELECT`
2. `FROM`
3. `WHERE`
4. `GROUP BY`
5. `ORDER BY`

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

Check out the next lesson: [Counting the unique values in a column](/intermediate/distinct.html)
