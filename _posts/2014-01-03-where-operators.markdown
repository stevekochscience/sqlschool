---
layout: sqlschool-lesson
category: "the-basics"
title:  "Filtering Data and Making Simple Calculations"
date:   2014-01-01 00:00:57
---

This lesson will use the same housing development data from the [previous lesson](/the-basics/select-from.html).

Start by running this code to remind yourself what's in the table that will be used in this tutorial. Remember to switch over to Mode and run any of the code you see in the grey boxes to get a sense of what the output will look like.

    SELECT * FROM tutorial.us_housing_units_completed

###The WHERE Clause
So you know how to view some data using `SELECT` and `FROM`. The next step is filtering the data using the `WHERE` clause. Here's what it looks like:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month = 1

*Note: the clauses always need to be in this order:* `SELECT`, `FROM`, `WHERE`.

The `WHERE` clause works in a plain-English way: the above query does the same thing as `SELECT * FROM tutorial.us_housing_units_completed`, except that the results will only include rows where the `month` column contains the value **1**.

**This part is really important: SQL deals with filtering an entire row at a time. In Excel, you can sort or filter one column at a time. In SQL, if you filter something in one column, entire rows will be omitted.** The idea is that each row is one *data point* or *observation*, and all the information contained in that row belongs together.

You can filter your results in a number of ways using comparison and logical operators.

###Comparison Operators
The most basic way to filter data is using comparison operators. The easiest way to understand them is to start by looking at the list of them:

<table>
  <tr><td>Equal to</td><td class="right"><code>=</code></td></tr>
  <tr><td>Not equal to</td><td class="right"><code><></code> or <code>!=</code></td></tr>
  <tr><td>Greater than</td><td class="right"><code>></code></td></tr>
  <tr><td>Less than</td><td class="right"><code><</code></td></tr>
  <tr><td>Greater than or equal to</td><td class="right"><code>>=</code></td></tr>
  <tr><td>Less than or equal to</td><td class="right"><code><=</code></td></tr>
</table>

These make the most sense when applied to numerical columns. For example:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE west > 30

Try running that query with each of the operators in place of `>`. Try some values other than `30` to get a sense of how this works. When you're ready, try to answer these questions:

<div class="practice-prob">
  Did the West Region ever produce <em>more than 50,000</em> housing units in one month? Remember, the units in the table are already in thousands.
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/eb5326c39637" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Did the South Region ever produce <em>20,000 or fewer</em> housing units in one month?
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/00e816870a30" target="_blank">See the Answer &raquo;</a>
</div>

###Comparisons on non-numerical data
All of the above operators work on non-numerical data as well. `=` and `!=` make perfect sense &mdash; they allow you to select rows that match or don't match any value, respectively. For example, run the following query and you'll notice that none of the January rows show up:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month_name != 'January'

There are some important rules when using these operators, though. If you're using an operator with values that are non-numeric, you need to put the value in single quotes: `'value'`. *Note: SQL uses single quotes (as opposed to double quotes) in every instance in reference to column names as identified in the [previous lesson](/the-basics/select-from.html).*

You can use `>`, `<`, and the rest of the operators on non-numeric columns as well &mdash; they filter based on alphabetical order. Try running this a couple times with different operators:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month_name > 'January'

If you're using `>`, `<`, `>=`, or `<=`, you don't necessarily need to be too specific about how you filter. Try this:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month_name > 'J'

The way SQL treats alphabetical ordering is a little bit tricky. You may have noticed in the above query that selecting `month_name > 'J'` will yield only rows in which `month_name` starts with "j" or later in the alphabet. "Wait a minute," you might say. "January is included in the results &mdash; shouldn't I have to use `month_name >= 'J` to make that happen?" SQL considers 'Ja' to be greater than 'J' because it has an extra letter. It's worth noting that most dictionaries would list 'Ja' after 'J' as well.

<div class="practice-prob">
  Write a query that only shows rows for which the month name is February.
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/1b0ec94376a6" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
 Write a query that only shows rows for which the <code>month_name</code> starts with the letter "M" or an earlier letter in the alphabet.
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/122ce812e03f" target="_blank">See the Answer &raquo;</a>
</div>

###Arithmetic
You can perform arithmetic in SQL using the same operators you would in Excel: `+`, `-`, `*`, `/`. SQL is a little tricky in that these only work in a given row. To clarify, you can only add values in multiple columns **from the same row** together using `+` &mdash; if you want to add values across multiple rows, you'll need to use [Aggregation Functions](/intermediate-sql/aggregation-functions.html), which are covered in the Intermediate Tutorial.

The example below illustrates the use of `+`:

    SELECT year,
           month,
           west,
           south,
           west + south AS south_plus_west
      FROM tutorial.us_housing_units_completed

The above example produces a column showing the sum of whatever is in the `south` and `west` columns for each row. You can chain arithmetic functions, including both column names and actual numbers:

    SELECT year,
           month,
           west,
           south,
           west + south - 4 * year AS nonsense_column
      FROM tutorial.us_housing_units_completed

The columns that contain the arithmetic functions are called "derived columns" because they are generated by modifying the information that exists in the underlying data.

<div class="practice-prob">
  Write a query that calculates the sum of all four regions in a separate column.
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/b83cefc05943" target="_blank">See the Answer &raquo;</a>
</div>

As in Excel, you can use parenthesis to manage the [order of operations](http://www.mathgoodies.com/lessons/vol7/order_operations.html). For example, if you wanted to average the `west` and `south` columns, you could write something like this:

    SELECT year,
           month,
           west,
           south,
           (west + south)/2 AS south_west_avg
      FROM tutorial.us_housing_units_completed

It occasionally makes sense to use parentheses even when it's not absolutely necessary just to make your query a bit easier to read. 

###Practice time
Sharpen your skills by tackling these questions:

<div class="practice-prob">
  Write a query that returns all rows for which more units were produced in the West region than in the Midwest and Northeast combined.
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/bb17c0984edb" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
 Write a query that calculates the percentage of all houses completed in the United States represented by each region. Only return results from the year 2000 and later. Hint: There should be four columns of percentages.
</div>
<div class="practice-prob-answer">
  <a href="http://stealth.modeanalytics.com/tutorial/reports/bffb59fa65a0" target="_blank">See the Answer &raquo;</a>
</div>

Move on to the next lesson: [Logical Operators](/the-basics/logical-operators.html).
