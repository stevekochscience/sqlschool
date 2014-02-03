---
layout: sqlschool-lesson
category: "the-basics"
title:  "Retrieving Data From the Database"
date:   2014-01-01 00:00:58
---

###Opening Mode; Getting Started
If you haven't already, open a second browser window to <a href="http://stealth.modeanalytics.com" target="_blank">Mode</a>. You will arrive at the homepage. You can use the homepage to explore the community or to quickly access a project you were working on previously, but since you probably haven't used Mode yet, click "New Query" to get started.

![Create a New Query](/images/the-basics/home-screen.png)

This will take you to the Query Builder. This is the bread and butter of Mode &mdash; it's where you'll test all of the skills you learn in this tutorial. Along those lines, every time you see a grey box like this, you should copy it into the query builder and click "run" to execute the query:

    SELECT * FROM tutorial.us_housing_units_completed

Here's what that looks like in practice:

<!--screenshot with that query in browser, ready to click run-->

It takes a few seconds for the query to run. When it's done, you will see the results show up in a table below the query window.

In this lesson, you'll work with real data from the US Census showing completed housing units in major regions. Each column represents a region, and the values represent the number of housing units completed in thousands. The data was collected on Jan. 31, 2014 and can be accessed at [the US Census website](http://www.census.gov/econ/currentdata/).

###Basic Syntax
There are two required ingredients in any SQL query: a `SELECT` statement and a `FROM` statement &mdash; and they have to be in that order. `SELECT` indicates which columns you'd like to view, and `FROM` identifies the table that they live in. Let's start by looking at a couple columns from the housing unit table:

    SELECT year, month, west FROM tutorial.us_housing_units_completed

*Don't forget to run that query!*

In this case, the query is telling the database to return the `year`, `month`, and `west` columns from the table `tutorial.us_housing_units_completed`. When you run this, you'll get back a set of results that shows values in each of these columns.

<!-- screenshot results  -->

Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should **not** include a comma after the last column name.

If you want to select every column in a table, you can use `*` instead of the column names:

    SELECT * FROM tutorial.us_housing_units_completed

<!-- screenshot results  -->

Now try this practice problem for yourself:

<div class="practice-prob">
  Write a query to select all of the columns in the <code>tutorial.us_housing_units_completed</code> table without using <code>*</code>.
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

*Note: Practice problems will appear in white boxes as above throughout this tutorial.*

When you've completed the above practice problem, check your answer by following the link below. You'll land on Mode's Report View &mdash; a cleaned-up view of a query meant for sharing. You can learn all about the report view on [Mode's help site](LINK), but for now, here's what you need to know:

1. Following the link will bring you to the Results page of the Report View. The results should look like the results your query produced.
2. If your results don't match (or even if they do), you can check out the answer query by clicking the "Query" tab in the Report View.
3. If you're feeling particularly proud of your work, you can create this report view to share with anyone by clicking the "Share" icon in the query builder window (see screenshot below).

<!-- screenshot highlighting share button-->

*Copy the URL in the sharing menu and send to all your friends!*

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

<div class="practice-prob">
  Write a query to select all of the columns in <code>tutorial.us_housing_units_completed</code> and rename them so that their first letters are capitalized
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

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

<div class="practice-prob">
  Write a query that uses <code>LIMIT</code> manually (written into the query) to restrict the result set to only 15 rows
</div>
<div class="practice-prob-answer">
  <a href="LINK">See the Answer &raquo;</a>
</div>

Now that you've got the basics, let's move on to some commands that will allow you to filter the data.

Next: [Filtering Data and Making Simple Calculations](/the-basics/where-operators.html).