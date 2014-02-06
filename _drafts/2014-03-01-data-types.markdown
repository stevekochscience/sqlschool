---
layout: sqlschool-lesson
category: "advanced"
title:  "Data Types and Expressions"
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


* CAST

###Crazy Rules for Dates/Times
* date functions, INTERVAL, and how numerical operators work on dates



http://www.firstsql.com/tutor3.htm#exp

