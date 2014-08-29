---
layout: sqlschool-lesson
category: "advanced"
title:  "Data Types"
date:   2014-03-01 00:00:59
seo-title: "Dealing with Data Types"
description: "Learn about SQL data types and how to convert between them with real-world examples. Free, interactive SQL tutorials to develop your data analysis skills."
---

Welcome to the Advanced SQL Tutorial! If you skipped [The Basics](/the-basics/basic-concepts), you should take a quick peek [at this page](/the-basics/select-from.html) to get an idea of how to get the most out of this tutorial. For convenience, here's the gist:

* Open another window to [Mode](http://modeanalytics.com). Make an account if you don't have one.
* For each lesson, start by running `SELECT *` on the relevant dataset so you get a sense of what the raw data looks like. Do this in that window you just opened to Mode.
* Run all of the code blocks in the lesson in Mode in the other window. You'll learn more if you really examine the results and understand what the code is doing.

For this lesson, we will use the same [Crunchbase data](http://info.crunchbase.com/about/crunchbase-data-exports/) from the [Outer Joins lesson](/intermediate/outer-joins.html). It was collected on Feb 15, 2014, and large portions of the data were dropped for the sake of this tutorial. In this example, we will also use a modified version of this data with date formats cleaned up to work better with SQL.

###Data Types
In previous lessons, you learned that certain functions work on some data types, but not others. For example, `COUNT` works with any data type, but `SUM` only works for numerical data (if this doesn't sound familiar, you should [revisit this lesson](/intermediate/aggregation-functions.html)). This is actually more complicated than it appears: in order to use `SUM`, the data must appear to be numeric, but it must also be **stored in the database** in a numeric form.

You might run into this, for example, if you have a column that appears to be entirely numeric, but happens to contain spaces or commas. Yes, it turns out that numeric columns cannot contain commas &mdash; If you upload data to Mode with commas in a column full of numbers, Mode will treat that column as non-numeric. Generally, numeric column types in various SQL databases *do not* support commas or currency symbols. To make things more complicated, SQL databases can store data in many different formats with different levels of precision.

The `INTEGER` data type, for example, only stores whole numbers &mdash; no decimals. The `DOUBLE PRECISION` data type, on the other hand, can store [between 15 and 17 significant decimal digits](http://en.wikipedia.org/wiki/Double-precision_floating-point_format) (almost certainly more than you need unless you're a physicist). There are a lot of data types, so it doesn't make sense to list them all here. For the complete list, [click here](http://www.w3schools.com/sql/sql_datatypes_general.asp). Here is the list of exact data types stored in Mode. "Imported as" refers to the types that is selected in the import flow (see image below), "Stored as" refers to the official SQL data type, and the third column explains the rules associated with the SQL data type:

<img src="/images/advanced/import_data_types.png" alt="{{ page.seo-title }}" title="{{ page.seo-title }}">

<table>
  <tr>
    <th width="20%">Imported as</th> 
    <th width="20%">Stored as</th>
    <th width="60%" class="right">With these rules</th>
  </tr>
  <tr>
    <td>String</td>
    <td>VARCHAR(1024)</td>
    <td class="right">Any characters, with a maximum field length of 1024 characters.</td>
  </tr>
  <tr>
    <td>Date/Time</td>
    <td>TIMESTAMP</td>
    <td class="right">Stores year, month, day, hour, minute and second values as YYYY-MM-DD hh:mm:ss.</td>
  </tr>
  <tr>
    <td>Number</td>
    <td>DOUBLE PRECISION</td>
    <td class="right">Numerical, with up to 17 significant digits decimal precision.</td>
  </tr>
  <tr>
    <td>Boolean</td>
    <td>BOOLEAN</td>
    <td class="right">Only TRUE or FALSE values.</td>
  </tr>
</table>

###Changing a Column's Data Type
It's certainly best for data to be stored in its optimal format from the beginning, but if it isn't, you can always change it in your query. It's particularly common for dates or numbers, for example, to be stored as strings. This becomes problematic when you want to sum a column and you get an error because SQL is reading numbers as strings. When this happens, you can use `CAST` or `CONVERT` to change the data type to a numeric one that will allow you to perform the sum.

<div id="cast"></div>
You can actually achieve this with two different type of syntax. The following examples all produce the same result:

* `CAST(column_name AS integer)`
* `column_name::integer`

You could replace `integer` with any other data type that would make sense for that column &mdash; all values in a given column must fit with the new data types.

<div class="practice-prob">
  Convert the <code>funding_total_usd</code> and <code>founded_at_clean</code> columns in the <code>tutorial.crunchbase_companies_clean_date</code> table to strings (varchar formar) using a different formatting function for each one.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/7387437bcb8c" target="_blank">See the Answer &raquo;</a>
</div>

Mode's public service (the thing you're using to complete this tutorial) performs implicit conversion in [certain circumstances](http://docs.aws.amazon.com/redshift/latest/dg/r_Type_conversion.html), so data types are rarely likely to be problematic. However, if you're accessing an external database (your employer's, for example), you may need to be careful about managing data types for some functions.

<!-- how you can lose data when converting between different data types: viz quickstart p124-->

###Why Dates are Formatted Year-First
If you live in the United States, you're probably used to seeing dates formatted as MM-DD-YYYY or a similar, month-first format. It's an odd convention [compared to the rest of the world's standards](http://www.theguardian.com/news/datablog/2013/dec/16/why-do-americans-write-the-month-before-the-day), but it's not necessarily any worse than DD-MM-YYYY. The problem with both of these formats is that when they are stored as strings, they don't sort in chronological order. For example, here's a date field stored as a string. Because the month is listed first, the `ORDER BY` statement doesn't produce a chronological list:

    SELECT permalink,
           founded_at
      FROM tutorial.crunchbase_companies_clean_date
     ORDER BY founded_at

You might think that converting these values from `string` to `date` might solve the problem, but it's actually not quite so simple. Mode (and most relational databases) format dates as YYYY-MM-DD, a format that makes a lot of sense because it will sort in the same order whether it's stored as a date or as a string. Excel is notorious for producing date formats that don't play nicely with other systems, so if you're exporting Excel files to CSV and uploading them to Mode, you may run into this a lot.

Here's an example from the same table, but with a field that has a cleaned date. Note that the cleaned date field is actually stored as a string, but still sorts in chronological order anyway:

    SELECT permalink,
           founded_at,
           founded_at_clean
      FROM tutorial.crunchbase_companies_clean_date
     ORDER BY founded_at

The lesson on [data cleaning](/advanced/data-cleaning.html) provides some examples for converting poorly formatted dates into proper date-formatted fields.

###Crazy Rules for Dates/Times
Assuming you've got some dates properly stored as a `date` or `time` data type, you can do some pretty powerful things. Maybe you'd like to calculate a field of dates a week after an existing field. Or maybe you'd like to create a field that indicates how many days apart the values in two other date fields are. These are trivially simple, but it's important to keep in mind that the data type of your results will depend on exactly what you are doing to the dates.

When you perform arithmetic on dates (such as subtracting one date from another), the results are often stored as the `interval` data type &mdash; a series of integers that represent a period of time. The following query uses date subtraction to determine how long it took companies to be acquired (unacquired companies and those without dates entered were filtered out). Note that because the `companies.founded_at_clean` column is stored as a string, it must be cast as a timestamp before it can be subtracted from another timestamp.

    SELECT companies.permalink,
           companies.founded_at_clean,
           acquisitions.acquired_at_cleaned,
           acquisitions.acquired_at_cleaned -
             companies.founded_at_clean::timestamp AS time_to_acquisition
      FROM tutorial.crunchbase_companies_clean_date companies
      JOIN tutorial.crunchbase_acquisitions_clean_date acquisitions
        ON acquisitions.company_permalink = companies.permalink
     WHERE founded_at_clean IS NOT NULL

In the example above, you can see that the `time_to_acquisition` column is an interval, not another date.

You can introduce intervals using the `INTERVAL` function as well:

    SELECT companies.permalink,
           companies.founded_at_clean,
           companies.founded_at_clean::timestamp +
             INTERVAL '1 week' AS plus_one_week
      FROM tutorial.crunchbase_companies_clean_date companies
     WHERE founded_at_clean IS NOT NULL

The interval is defined using plain-English terms like '10 seconds' or '5 months'. Also note that adding or subtracting a `date` column and an `interval` column results in another `date` column as in the above query.

<!--
If you're interested in digging deeper into all of the ways you can manipulate dates, this is covered in greater detail [here](/solutions-to-common-problems/working-with-dates-times.html). You can also check out the [Postgres syntax guide](http://www.tutorialspoint.com/postgresql/postgresql_date_time.htm).
-->
<div class="practice-prob">
  Write a query that counts the number of companies acquired within 3 year, 5 years, and 10 years or founding (in 3 separate columns). Include a column for total companies acquired as well. Group by category and limit to only rows with a founding date.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/3043c5879728" target="_blank">See the Answer &raquo;</a>
</div>

You can add the current time (at the time you run the query) into your code using the `NOW()`function:

    SELECT companies.permalink,
           companies.founded_at_clean,
           NOW() - companies.founded_at_clean::timestamp AS founded_time_ago
      FROM tutorial.crunchbase_companies_clean_date companies
     WHERE founded_at_clean IS NOT NULL
