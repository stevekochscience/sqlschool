---
layout: sqlschool-lesson
category: "the-basics"
title:  "Filtering Data and Making Simple Calculations"
date:   2014-01-01 00:00:57
---

This lesson will use the same housing development data from the [previous lesson](/the-basics/select-from.html)

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

Try running that query with each of the operators in place of `>`. Try some values other than `30` to get a sense of how this works. When you're ready, try to answer thse questions:

<div class="practice-prob">
  Did the West Region ever produce <em>more than 50,000</em> housing units in one month? Remember, the units in the table are already in thousands.
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Did the South Region ever produce <em>20,000 or fewer</em> housing units in one month?
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

###Comparisons on non-numerical data
All of the above operators work on non-numerical data as well. `=` and `!=` make perfect sense &mdash; they allow you to select rows that match or don't match any value, respectively. For example, run the following query and you'll notice that none of the January rows show up:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month_name != 'January'

There are some important rules when using these operators, though. If you're using an operator with values that are non-numeric, you need to put the value in single quotes: `'value'`. *Note: SQL uses single quotes (as opposed to double quotes) in every instance except one, which we will get to in a later tutorial.*

You can use `>`, `<`, and the rest of the operators on non-numeric columns as well &mdash; they filter based on alphabetical order. Try running this a couple times with different operators:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month_name > 'January'

If you're using `>`, `<`, `>=`, or `<=`, you don't necessarily need to be too specific about how you filter. Try this:

    SELECT *
      FROM tutorial.us_housing_units_completed
     WHERE month_name > 'J'

The way SQL treats alphabetical ordering is a little bit tricky. You may have noticed in the above query that selecting `month_name > 'J'` will yield only rows in which fieldname starts with "n" or later in the alphabet. "Wait a minute," you might say. "January is included in the results &mdash; shouldn't I have to use `month_name >= 'J` to make that happen?" SQL considers 'Ja' to be greater than 'J' because it has an extra letter. It's worth noting that most dictionaries would list 'Ja' after 'J' as well.

<div class="practice-prob">
  Write a query that only shows rows from Q1 of a given year (Jan-Mar)
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
 Write a query that only shows rows from Q4 of a given year (Oct-Dec)
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

###Arithmetic
You can perform arithmetic in SQL using the same operators you would in Excel: `+`, `-`, `*`, `/`. SQL is a little tricky in that these only work in a given row. To clarify, you can only add values in multiple columns **from the same row** together using `+` &mdash; if you want to add values across multiple rows, you'll need to use [Aggregation Functions](/intermediate-sql/aggregation-functions.html), which are covered in the Intermediate Tutorial.

The example below illustrates the use of `+`:

    SELECT year,
           month,
           west,
           south,
           west + south
      FROM tutorial.us_housing_units_completed

<!--explanation of the above -->

<!-- example using another operator-->

<!-- parenthesis example -->

###Practice time
Sharpen your skills by tackling these questions:

* question 1
[answer 1](LINK)
* question 2
[answer 2](link)

Move on to the next lesson: [Logical Operators](/the-basics/logical-operators.html).
