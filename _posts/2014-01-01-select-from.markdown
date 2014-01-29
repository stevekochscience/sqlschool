---
layout: sqlschool-lesson
category: "the-basics"
title:  "SELECT and FROM"
date:   2014-01-01 00:00:58
---

#####Basic Syntax
There are two required ingredients in any SQL query: a `SELECT` statement and a `FROM` statement &mdash; and they have to be in that order. `SELECT` indicates which columns you'd like to view, and `FROM` identifies the table that they live in. Let's look at an example:

    SELECT salesperson, widget_sales FROM tutorial.sales_performance

In this case, the query is telling the database to return `column1` and `column2` from the table `mode.sample_table_1`. When you run this, you'll get back a set of results that shows every value in each of these columns.

* screenshot results

Note that the two column names were separated by a comma in the query. Whenever you select multiple columns, they must be separated by commas, but you should **not** include a comma after the last column name.

If you want to select every column in a table, you can use `*` instead of the column names:

    SELECT * FROM tutorial.sales_performance

* screenshot results

#####What does this actually do?
So when you run a query, what do you get back? As you can see from running the above queries, you get a table. But that table isn't stored permanently in the database. It also doesn't change any tables in the database &mdash; `mode.sample_table_1` will contain the same data every time you query it. Mode does store all of your results for future access, but the important part for the sake of this tutorial is that `SELECT` statements don't change anything in the underlying tables.

#####Formatting convention
You might have noticed that the `SELECT` and `FROM' commands are capitalized. This isn't actually necessary &mdash; SQL will understand these commands if you type them in lowercase. Capitalizing commands is simply a convention that makes queries easier to read. Similarly, SQL treats one space, multiple spaces, or a line break as being the same thing. For example, SQL treats this the same way it does the previous query:

    SELECT *        FROM tutorial.sales_performance

It also treats this the same way:

    SELECT *
    FROM tutorial.sales_performance

There are several conventions for formatting line breaks, some of which you'll pick up on through this tutorial and by simply checking out other people's work on Mode. It's up to you to determine what formatting elements are easiest for you to read and keep track of.

#####Column names
While we're on the topic of formatting, it's worth noting the format of column names. You may notice that all of the columns in the `mode.sample_table_1` table are named in lower case, and use underscores instead of spaces. Even the table name itself uses underscores instead of spaces. This is because it's annoying to deal with spaces in SQL &mdash; if you want to have spaces in column names, you need to always refer to those columns in double quotes. For example, if a column was named Column 1, I would have to type `SELECT "Column 1" FROM mode.sample_table_1`. Without the double quotes, that query would read 'Column' and '1' as separate objects and would return an error.

#####LIMIT
You may have noticed that checkbox next to the "Run" button that says "Limit."

* screenshot of limit.

The default value is 100; when this box is checked, it's effectively telling the database to only return 100 rows of this table. This particular table has fewer than 100 rows, so it's irrelevant. Try setting the limit to 5, though, and see what happens when you run the query.

This exists to keep your queries running quickly. You'll probably find that the aim of many of your queries will simply be to see what a particular table looks like &mdash; you'll want to scan the first few rows of data to get an idea of which fields you care about and how you want to manipulate them. If you query a very large table and don't use a limit, you could end up waiting a long time for your results to return, which doesn't make sense if you only care about the first few.

The limiting functionality is built into Mode, but if you're ever using SQL in another context, you can add a limit with a SQL command. The following syntax does the same thing as having the box checked with a value of 100:

    SELECT * FROM tutorial.sales_performance LIMIT 100

Now that you've got the basics, let's move on to some commands that will allow you to filter the data.

* LINK TO NEXT SEGMENT