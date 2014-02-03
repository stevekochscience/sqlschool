---
layout: sqlschool-lesson
category: "intermediate"
title:  "Summary Statistics: Using SQL to Replace Pivot Tables"
date:   2014-02-01 00:00:59
---

Welcome to the Intermediate SQL Tutorial! If you skipped [The Basics](LINK), you should take a quick peek [at this page](LINK) to get an idea of how to get the most out of this tutorial. For convenience, here's the gist:

* Open another window to [Mode](http://modeanalytics.com). Make an account if you don't have one.
* For each lesson, start by running `SELECT *` on the relevant dataset so you get a sense of what the raw data looks like. Do this in that window you just opened to Mode.
* Run all of the code blocks in the lesson in Mode in the other window. You'll learn more if you really examine the results and understand what the code is doing.

###Today's dataset

For this lesson and the next, you'll be working with [Apple](http://www.apple.com) stock price data. The data was pulled from [Google Finance](http://finance.google.com) in January 2014. There is one row for each day (indicated in the `date` field). `open` and `close` are the opening and closing prices of the stock on that day. `high` and `low` are the high and low prices for that day. `volume` is the number of shares traded on that day. Some data has been intentionally removed for the sake of this lesson. Check it out for yourself:

    SELECT * FROM tutorial.aapl_historical_stock_price

###Aggregation Functions
As the [beginner tutorial](/the-basics/basic-concepts.html) points out, SQL is excellent at aggregating data the way you might in a pivot table in Excel. You will use aggregation functions all the time, so it's important to get comfortable with them. The functions themselves are the same ones you will find in Excel or any other analytics program: `COUNT`, `SUM`, `MIN`, `MAX`, `AVG`. The [beginner tutorial](/the-basics/where-operators.html) also pointed out that arithmetic operators only perform operations across rows. Aggregation functions are used to perform operations across entire columns (could be millions of rows of data or more).

###COUNT
It's easiest to start with `COUNT` because verifying your results is extremely simply. The easiest way to get started is to use `*` to select all rows.

    SELECT COUNT(*)
      FROM tutorial.aapl_historical_stock_price

You can see that the result showed a count of all rows to be 3555. Turn the limit off and run the following query and note that Mode actually provides a row count, which should be the same as the result of the above query:

    SELECT * FROM tutorial.aapl_historical_stock_price

Things start to get a little bit tricky when you want to count individual columns. The following code will provide a count of all of rows in which the `high` column **is not null**.

    SELECT COUNT(high)
      FROM tutorial.aapl_historical_stock_price

You'll notice that this result is lower than what you got with `COUNT(*)`. That's because `high` has some nulls.

<div class="practice-prob">
  practice problem
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
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
  <a href="LINK">See the Answer &raquo;</a>
</div>

###SUM
This works the same way as count, except that it can only be used on numerical columns. As you might expect, `SUM` totals all the values in a given column:

    SELECT SUM(volume)
      FROM tutorial.aapl_historical_stock_price

An important thing to remember: aggregators only aggregate vertically. If you want to perform a calculation across rows, you would do this with [simple arithmetic](LINK TO EARLIER TUTORIAL).

You don't need to worry as much about the presence of nulls with `SUM` as you would with `COUNT` &mdash; `SUM` treats nulls as 0.

<div class="practice-prob">
  Write a query to calculate the average opening price (hint: you will need to use both `COUNT` and `SUM`).
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
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
  On what day did the stock price peak?
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  On what day did the stock increase by the highest dollar value?
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

###AVG
`AVG` does what you'd think &mdash; it calculated the average of selected values. It is very useful, but has some limitations. First, it can only be used on numerical columns. Second, it ignores nulls completely. There are some cases in which you will want to treat null values as 0. For these cases, you'll want to write a statement that changes the nulls to 0 (covered in a [later lesson](/intermediate/case.html)).

You can see this by comparing these two queries:

    SELECT AVG(volume)
      FROM tutorial.aapl_historical_stock_price
     WHERE volume IS NOT NULL

Produces the same result as:

    SELECT AVG(volume)
      FROM tutorial.aapl_historical_stock_price

<div class="practice-prob">
  practice problem
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

###GROUP BY
All of the queries above have something in common: they all aggregate across the entire table. What if you want to aggregate only part of the table &mdash; you want to count entries by month, for example. `GROUP BY` allows you to separate your aggregations into sub-groups. Here's an example:

    SELECT year,
           COUNT(*) as count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY year

You can group by multiple columns, but you have to separate column names with commas &mdash; just as with `ORDER BY`:

    SELECT year,
           month,
           COUNT(*) as count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY year, month

<div class="practice-prob">
  practice problem
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

As in [ORDER BY](/the-basics/order-by.html), you can substitute numbers for column names in the `GROUP BY` clause. It's generally recommended to do this only when you are grouping many columns, or if something else is causing the text in the `GROUP BY` clause to be excessively long:

    SELECT year,
           month,
           COUNT(*) as count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY 1, 2

*Note: this functionality (numbering columns instead of using names) is supported by Mode, but not by every flavor of SQL, so if you're using another system or connected to certain types of databases, it may not work.*

The order of column names in your `GROUP BY` clause doesn't matter &mdash; the results will be the same regardless. If you want to control how the aggregations are grouped together, use `ORDER BY`. Try running the query below, then reverse the column names in the `ORDER BY` statement and see how it looks:

    SELECT year,
           month,
           COUNT(*) as count
      FROM tutorial.aapl_historical_stock_price
     GROUP BY 1, 2
     ORDER BY 2, 1

There's one thing to be aware of as you group by multiple columns: SQL evaluates the aggregations before the `LIMIT` clause. If you don't group by any columns, you'll get a 1-row result &mdash; no problem there. If you group by a column with enough unique values that it exceeds the `LIMIT` number, what will happen is that aggregates will be calculated, and then some rows will simply be omitted from the results. This is actually a nice way to do things because you know you're going to get the correct aggregates. If SQL cut the table down to 100 rows, then performed the aggregations, your results would be substantially different. The above query's results exceed 100 rows, so it is a perfect example. Try removing the limit and running it again.

###Query Clause Order
As mentioned in prior tutorials, the order in which you write the clauses is important. Here's the order for everything you've learned so far:

1. `SELECT`
2. `FROM`
3. `WHERE`
4. `GROUP BY`
5. `ORDER BY`

###Aggregation Practice

<div class="practice-prob">
  Write a query to calculate the average difference between opening and closing prices
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  problem 2: something with group by
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  problem 3
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>


Check out the next lesson: [Counting the unique values in a column](/intermediate/distinct.html)
