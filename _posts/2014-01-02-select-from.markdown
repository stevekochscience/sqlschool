---
layout: sqlschool-lesson
category: "the-basics"
title:  "Retrieving Data From the Database"
date:   2014-01-01 00:00:58
---

###Opening Mode; Getting Started
<!--homepage stuff -->

<!--new query -->

In this lesson, you'll work with real data from the US Census showing completed housing units in major regions. Each column represents a region, and the values represent the number of housing units completed in thousands. The data was collected on Jan. 31, 2014 and can be accessed at [the US Census website](http://www.census.gov/econ/currentdata/).

<!--big note that the user should switch windows and run a query every time they see one in a grey box-->

###Basic Syntax
There are two required ingredients in any SQL query: a `SELECT` statement and a `FROM` statement &mdash; and they have to be in that order. `SELECT` indicates which columns you'd like to view, and `FROM` identifies the table that they live in. Let's start by looking at a couple columns from the housing unit table:

    SELECT year, month, west FROM tutorial.us_housing_units_completed

In this case, the query is telling the database to return the `year`, `month`, and `west` columns from the table `tutorial.us_housing_units_completed`. When you run this, you'll get back a set of results that shows values in each of these columns.

<!-- screenshot results  -->

Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should **not** include a comma after the last column name.

If you want to select every column in a table, you can use `*` instead of the column names:

    SELECT * FROM tutorial.us_housing_units_completed

<!-- screenshot results  -->

* practice problem: try selecting all of the columns without using "*"

<!--talk about report view-->
<link to report view of answer>

###What actually happens when you run a query?
So when you run a query, what do you get back? As you can see from running the above queries, you get a table. But that table isn't stored permanently in the database. It also doesn't change any tables in the database &mdash; `mode.sample_table_1` will contain the same data every time you query it. Mode does store all of your results for future access, but the important part for the sake of this tutorial is that `SELECT` statements don't change anything in the underlying tables.

###Formatting convention
You might have noticed that the `SELECT` and `FROM' commands are capitalized. This isn't actually necessary &mdash; SQL will understand these commands if you type them in lowercase. Capitalizing commands is simply a convention that makes queries easier to read. Similarly, SQL treats one space, multiple spaces, or a line break as being the same thing. For example, SQL treats this the same way it does the previous query:

    SELECT *        FROM tutorial.us_housing_units_completed

It also treats this the same way:

    SELECT *
    FROM tutorial.us_housing_units_completed

There are several conventions for formatting line breaks, some of which you'll pick up on through this tutorial and by simply checking out other people's work on Mode. It's up to you to determine what formatting elements are easiest for you to read and keep track of.

###Column names
While we're on the topic of formatting, it's worth noting the format of column names. You may notice that all of the columns in the `tutorial.us_housing_units_completed` table are named in lower case, and use underscores instead of spaces. Even the table name itself uses underscores instead of spaces. This is because it's annoying to deal with spaces in SQL &mdash; if you want to have spaces in column names, you need to always refer to those columns in double quotes.

If you'd like your results to look a bit more presentable, you can rename columns to include spaces. For example, if you want the `west` column to appear as `West Region` in the results, you would have to type:

    SELECT west AS "West Region" FROM tutorial.us_housing_units_completed

Without the double quotes, that query would read 'West' and 'Region' as separate objects and would return an error.

* practice problem: try selecting all of the columns in tutorial.us_housing_units_completed and renaming them so that their first letters are capitalized
<link to answer>

###LIMIT
You may have noticed that checkbox next to the "Run" button that says "Limit."

<!-- screenshot of limit. -->

The default value is 100; when this box is checked, it's effectively telling the database to only return 100 rows of this table. This particular table has more than 100 rows, so your queries thus far have not been returning the full result sets. Try turning the `LIMIT` off (by clicking the check mark next to it) and running this query. 

    SELECT * FROM tutorial.us_housing_units_completed

You'll notice many more rows get returned. You can tell by the id field in the table, which increases by 1 for each row.
<!--screenshot of id field-->

This exists to keep your queries running quickly. You'll probably find that the aim of many of your queries will simply be to see what a particular table looks like &mdash; you'll want to scan the first few rows of data to get an idea of which fields you care about and how you want to manipulate them. If you query a very large table and don't use a limit, you could end up waiting a long time for your results to return, which doesn't make sense if you only care about the first few.

The limiting functionality is built into Mode, but if you're ever using SQL in another context, you can add a limit with a SQL command. The following syntax does the same thing as having the box checked with a value of 100:

    SELECT * FROM tutorial.sales_performance LIMIT 100

* Practice Problem: use LIMIT manually (written into the query) to restrict the result set to only 15 rows
<link to answer>

Now that you've got the basics, let's move on to some commands that will allow you to filter the data.

Next: [WHERE and Comparison Operators](/the-basics/where-operators.html).