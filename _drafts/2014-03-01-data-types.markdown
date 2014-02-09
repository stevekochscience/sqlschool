---
layout: sqlschool-lesson
category: "advanced"
title:  "Data Types"
date:   2014-03-01 00:00:59
---

Welcome to the Advanced SQL Tutorial! If you skipped [The Basics](/the-basics/basic-concepts), you should take a quick peek [at this page](/the-basics/select-from.html) to get an idea of how to get the most out of this tutorial. For convenience, here's the gist:

* Open another window to [Mode](http://modeanalytics.com). Make an account if you don't have one.
* For each lesson, start by running `SELECT *` on the relevant dataset so you get a sense of what the raw data looks like. Do this in that window you just opened to Mode.
* Run all of the code blocks in the lesson in Mode in the other window. You'll learn more if you really examine the results and understand what the code is doing.

###Data Types
In previous lessons, you learned that certain functions work on some data types, but not others. For example, `COUNT` works with any data type, but `SUM` only works for numerical data (if this doesn't sound familiar, you should [revisit this lesson](/intermediate/aggregation-functions.html)). This is actually more complicated than it appears: in order to use `SUM`, the data must appear to be numeric, but it must also be **stored in the database** in a numeric form.

You might run into this, for example, if you have a column that appears to be entirely numeric, but happens to contain spaces or commas. Yes, it turns out that numeric columns cannot contain commas &mdash; If you upload data to Mode with commas in a column full of numbers, Mode will treat that column as non-numeric. Generally, numeric column types in various SQL databases *do not* support commas or currency symbols. To make things more complicated, SQL databases can store data in many different formats with different levels of precision.

The `INTEGER` data type, for example, only stores whole numbers &mdash; no decimals. The `DOUBLE PRECISION` data type, on the other hand, can store [between 15 and 17 significant decimal digits](http://en.wikipedia.org/wiki/Double-precision_floating-point_format) (almost certainly more than you need unless you're a physicist). There are a lot of data types, so it doesn't make sense to list them all here. For the complete list, [click here](http://www.w3schools.com/sql/sql_datatypes_general.asp). Here is the list of exact data types stored in Mode. "Imported as" refers to the types that is selected in the import flow (see image below), "Stored as" refers to the official SQL data type, and the third column explains the rules associated with the SQL data type:

<!-- screenshow showing dropdown selection for data type in import flow-->

<table>
  <tr><th>Imported as</th>  <th>Stored as</th>        <th>With these rules</th></tr>
  <tr><td>Number</td>       <td>DOUBLE PRECISION</td> <td class="right">RULES</td></tr>
  <tr><td>Date/Time</td>    <td>TIMESTAMP</td>        <td class="right">RULES</td></tr>
  <tr><td>String</td>       <td>VARCHAR(1024)</td>    <td class="right">RULES</td></tr>
  <tr><td>Boolean</td>      <td>BOOLEAN</td>          <td class="right">RULES</td></tr>
</table>

###Changing a Column's Data Type
It's certainly best for data to be stored in its optimal format from the beginning, but if it isn't, you can always change it in your query. It's particularly common for dates or numbers, for example, to be stored as strings. This becomes problematic when you want to sum a column and you get an error because SQL is reading numbers as strings. When this happens, you can use `CAST` or `CONVERT` to change the data type to a numeric one that will allow you to perform the sum.

You can actually achieve this with three different type of syntax. The following examples all produce the same result:

* `CAST(column_name AS integer)`
* `CONVERT(column_name, integer)`
* `column_name::integer`

You could replace `integer` with any other data type that would make sense for that column &mdash; all values in a given column must fit with the new data types.

<div class="practice-prob">
  CAST practice
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>

Mode's public service (the thing you're using to complete this tutorial) performs implicit conversion in [certain circumstances](http://docs.aws.amazon.com/redshift/latest/dg/r_Type_conversion.html), so data types are rarely likely to be problematic. However, if you're accessing an external database (your employer's, for example), you may need to be careful about managing data types for some functions.

###Why Dates are Formatted Year-First


###Crazy Rules for Dates/Times
Maybe you'd like to calculate a field of dates a week after an existing field. Or maybe you'd like to create a field that indicates how many days apart the values in two other date fields are. When you perform arithmetic on dates, the results are often stored as the `interval` data type &mdash; a series of integers that represent a period of time.

<!--example that casts a field as an interval, then does some addition -->

You can introduce intervals using the `INTERVAL` function as well:

    SELECT date
    date + INTERVAL('1 week')
      FROM table

The interval is defined using plain-English terms like '10 seconds' or '5 months'. Also note that adding or subtracting a `date` column and an `interval` column results in another `date` column:

<!-- date functions, INTERVAL, and how numerical operators work on dates-->

If you subtract one date column from another, you get an `interval` column:

<!--example of above concept -->

<div class="practice-prob">
  CAST practice
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>



