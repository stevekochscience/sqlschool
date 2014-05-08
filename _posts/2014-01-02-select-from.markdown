---
layout: sqlschool-lesson
category: "the-basics"
title:  "Retrieving Data From the Database"
date:   2014-01-01 00:00:58
---

###Opening Mode and Getting Started
If you're not yet signed up for Mode, [drop us an email to be added to the private beta](mailto:info@modeanalytics.com).

If you haven't already, open a second browser window to <a href="https://modeanalytics.com" target="_blank">Mode</a> and log in. You'll arrive at your homepage. You can use the homepage to explore the community or to quickly access a project you were working on previously. Since you probably haven't used Mode yet, click "New Query" to get started.

![Create a New Query](/images/the-basics/new-query.png)

This will take you to the Query Builder. This is the bread and butter of Mode&mdash;it's where you'll be able to use all the skills you learn in this tutorial. From the Query Builder, you can run queries against all of the data in Mode. 

For this tutorial, SQL queries will be shown in grey boxes like the one below. To run a query, simply copy the text from the box into the Query Builder and click the orange "Run Query" button. Alternatively, you can run a query by pressing <code>&#8984;</code> + `return` on a Mac or `ctrl` + `return` on a PC.

    SELECT * 
      FROM tutorial.us_housing_units_completed

Give this a shot, then keep reading to learn what the query is doing.

![Run the Query](/images/the-basics/run-button.png)

It takes a few seconds for the query to run. When it's done, you'll see the results show up in a table below the query window.

In this first lesson, you'll work with real data from the U.S. Census. This dataset shows the number of completed housing units in major regions of the United States. The table we'll be working with has a column for each region. The values in each row represent the number of housing units completed in thousands during the corresponding month. The data was collected on Jan. 31, 2014 and can be accessed at [the U.S. Census website](http://www.census.gov/econ/currentdata/).

###Basic Syntax
There are two required ingredients in any SQL query: a `SELECT` statement and a `FROM` statement&mdash;and they have to be in that order. `SELECT` indicates which columns you'd like to view, and `FROM` identifies the table that they live in. Let's start by looking at a couple columns from the housing unit table:

    SELECT year, 
           month, 
           west 
      FROM tutorial.us_housing_units_completed

To see the results yourself, copy and paste this query into a Query Builder and click run! If you ran the previous query, or if you already have text in Mode's query window, you'll need to paste over or delete the query that was there previously. If you simply copy and paste this query below the previous one, you'll get an error &mdash; you can only run one `SELECT` statement at a time.

<!--image "do this, not this"-->

So what's happening in the above query? In this case, the query is telling the database to return the `year`, `month`, and `west` columns from the table `tutorial.us_housing_units_completed`. (Remember that when referencing tables, the table names have to be preceded by [the name of user who uploaded it](/the-basics/basic-concepts.html).) When you run this query, you'll get back a set of results that shows values in each of these columns.

![Prelimiary Results](/images/the-basics/prelim-results.png)

Note that the three column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should **not** include a comma after the last column name.

If you want to select every column in a table, you can use `*` instead of the column names:

    SELECT * 
      FROM tutorial.us_housing_units_completed

![Query Results](/images/the-basics/results.png)

Now try this practice problem for yourself:

<div class="practice-prob">
  Write a query to select all of the columns in the <code>tutorial.us_housing_units_completed</code> table without using <code>*</code>.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/cc50612804ae" target="_blank">See the Answer &raquo;</a>
</div>

*Note: Practice problems will appear in white boxes as above throughout this tutorial.*

When you've completed the above practice problem, check your answer by following the link below. You'll land on Mode's Report View&mdash;a cleaned-up view of a query meant for sharing queries and results. You can learn more about the report view on [Mode's help site](LINK), but for now, here's what you need to know:

1. Following the link will bring you to the Results page of the Report View. The results should look like the results your query produced.
2. If your results don't match (or even if they do), you can check out the answer query by clicking the "Query" tab in the Report View.
3. If you're feeling particularly proud of your work, you can create this report view to share with anyone by clicking the "Share" icon in the query builder window (see screenshot below).

![Sharing is Caring](/images/the-basics/how-to-share.png)

*Copy the URL in the share menu and send to all your friends!*

You can share your work via URL at any state, but if you want to polish your work up a little by adding a title and description, click the "Save" button, complete the fields, and click "Save Report."

![Saving your Work](/images/the-basics/saving-work.png)

###What actually happens when you run a query?
When you run a query, what do you get back? As you can see from running the queries above, you get a table. But that table isn't stored permanently in the database. It also doesn't change any tables in the database&mdash;`tutorial.us_housing_units_completed` will contain the same data every time you query it, and the data will never change no matter how many times you query it. Mode does store all of your results for future access, but `SELECT` statements don't change anything in the underlying tables.

###Formatting convention
You might have noticed that the `SELECT` and `FROM' commands are capitalized. This isn't actually necessary&mdash;SQL will understand these commands if you type them in lowercase. Capitalizing commands is simply a convention that makes queries easier to read. Similarly, SQL treats one space, multiple spaces, or a line break as being the same thing. For example, SQL treats this the same way it does the previous query:

    SELECT *        FROM tutorial.us_housing_units_completed

It also treats this the same way:

    SELECT *
      FROM tutorial.us_housing_units_completed

While most capitalization conventions are the same, there are several conventions for formatting line breaks. You'll pick up on several of these in this tutorial and in other people's work on Mode. It's up to you to determine what formatting method is easiest for you to read and understand.

###Column names
While we're on the topic of formatting, it's worth noting the format of column names. All of the columns in the `tutorial.us_housing_units_completed` table are named in lower case, and use underscores instead of spaces. The table name itself also uses underscores instead of spaces. Most people avoid putting spaces in column names because it's annoying to deal with spaces in SQL&mdash;if you want to have spaces in column names, you need to always refer to those columns in double quotes.

If you'd like your results to look a bit more presentable, you can rename columns to include spaces. For example, if you want the `west` column to appear as `West Region` in the results, you would have to type:

    SELECT west AS "West Region" 
      FROM tutorial.us_housing_units_completed

Without the double quotes, that query would read 'West' and 'Region' as separate objects and would return an error. Note that the results will only return capital letters if you put column names in double quotes. The following query, for example, will return results with lower-case column names.

    SELECT west AS West_Region,
           south AS South_Region
      FROM tutorial.us_housing_units_completed

<div class="practice-prob">
  Write a query to select all of the columns in <code>tutorial.us_housing_units_completed</code> and rename them so that their first letters are capitalized
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/740ad94d2ef9" target="_blank">See the Answer &raquo;</a>
</div>

###LIMIT
You may have noticed that checkbox next in the upper-right portion of the editor that says "Limit."

![Limit](/images/the-basics/limit-box.png)

As you might expect, the limit restricts how many rows the SQL query returns. The default value is 100; when this box is checked, it's telling the database to only return the first 100 rows of the query. Because `tutorial.us_housing_units_completed` has more than 100 rows, the queries thus far haven't been returning the full result sets. Try turning the `LIMIT` off (by clicking the check mark next to it) and running this query. 

    SELECT * 
      FROM tutorial.us_housing_units_completed

You'll notice many more rows get returned. You can tell by the `id` field in the table, which increases by 1 for each row.

![ID Field](/images/the-basics/id-field.png)

Many analysts use limits as a simple way to keep their queries from taking too long to return. You'll probably find that the aim of many of your queries will simply be to see what a particular table looks like&mdash;you'll want to scan the first few rows of data to get an idea of which fields you care about and how you want to manipulate them. If you query a very large table (such as one with hundreds of thousands or millions of rows) and don't use a limit, you could end up waiting a long time for all of your results to be displayed, which doesn't make sense if you only care about the first few. 

The limiting functionality is built into Mode to prevent you from accidentally returning millions of rows without meaning to (we've all done it). If you're ever using SQL in another context, however, you can add a limit with a SQL command. The following syntax does the same thing as having the box checked with a value of 100:

    SELECT * 
      FROM tutorial.us_housing_units_completed 
     LIMIT 100

<div class="practice-prob">
  Write a query that uses <code>LIMIT</code> manually (written into the query) to restrict the result set to only 15 rows
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/62f423b84e97" target="_blank">See the Answer &raquo;</a>
</div>

Congrats on learning the basics! Let's now move on to some commands that will allow you to filter the data.

Next: [Filtering Data and Making Simple Calculations](/the-basics/where-operators.html).