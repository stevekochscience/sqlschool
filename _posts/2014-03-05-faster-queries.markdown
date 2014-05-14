---
layout: sqlschool-lesson
category: "advanced"
title:  "Making Queries Run Faster"
date:   2014-03-01 00:00:55
---

The lesson on [subqueries](/advanced/subqueries.html) introduced the idea that you can sometimes create the same desired result set with a faster-running query. In this lesson, you'll learn to identify when your queries can be improved, and how to improve them.

###The Theory Behind Query Run Time
A database is a piece of software that runs on a computer, and is subject to the same limitations as all software &mdash; it can only process as much information as its hardware is capable of handling. The way to make a query run faster is to reduce the number of caluclations that the software (and therefore hardware) must perform. To do this, you'll need some understanding of how SQL actually makes calculations. First, let's address some of the high-level things that will affect the number of calculations you need to make, and therefore your querys runtime:

* **Table size:** If your query hits one or more tables with millions of rows or more, it could affect performance.
* **Joins:** If your query joins two tables in a way that substantially increases the row count of the result set, your query is likely to be slow. There's an example of this in the section on Joining Subqueries in the [Subqueries lesson](/advanced/suqueries.html).
* **Aggregations:** Combining multiple rows to produce a result requires more computation than simply retrieving those rows.

Query runtime is also dependent on some things that you can't really control related to the database itself:

* **Other users running queries:** The more queries running concurrently on a database, the more the database must process at a given time and the slower everything will run. It can be especially bad if others are running particularly resource-intensive queries that fulfill some of the above criteria.

* **Database software and optimization:** This is something you probably can't control, but if you know the system you're using, you can work within its bounds to make your queries more efficient.

For now, let's ignore the things you can't control and work on the things you can.

###Reducing table size
Filtering the data to include only the observations you need can dramatically improve query speed. How you do this will depend entirely on the problem you're trying to solve. For example, if you've got time series data, limiting to a small time window can make your queries run much more quickly:

<!-- use benn's example event data -->

Keep in mind that you can always perform exploratory analysis on a subset of data, refine your work into a final query, then remove the limitation and run your work across the entire dataset. The final query might take a long time to run, but at least you can run the intermediate steps quickly.

This is why Mode enforces a `LIMIT` clause by default &mdash; 100 rows is often more than you need to determine the next step in your analysis, and it's a small enough dataset that it will return quickly.

![Limit](/images/the-basics/limit-box.png)

If you're working with [subqueries](advanced/subqueries.html), you should make sure to limit the amount of data you're working with in the place where it will be executed first. For example, this query applies a limit within the subquery:

<!-- limit in subquery example -->

The following query applies the same limit to the outer query, and takes much longer to run:

<!-- limit in outer query -->

###Making joins less complicated
In a way, this is an extension of the previous tip. In the same way that it's better to reduce data at a point in the query that is executed early, it's better to reduce table sizes before joining them. Take this example, which uses <!--BENN"S DATA" -->

<!-- example: joining events table onto users table. bad example uses COUNT(CASE) to do counts on huge dataset. good example limits the events table before joining -->


###Optimizing aggregations

<!-->
###How Databases Work
There are many types of relational databases out there, all with nuances that make them better for certain specialized tasks. Generally, they fall into two large buckets:

* Row-oriented: Data is retrieved from a row-oriented database one row at a time, and all of the columns are retrieved for that row. This is an optimal way to store data that will be served to web applications, which often request individual rows at a time. Think back to the [Crunchbase data](http://info.crunchbase.com/about/crunchbase-data-exports/) you worked with in the [outer join lesson](/intermediate/outer-joins.html). If you were to [actually view a company profile on Crunchbase](http://www.crunchbase.com/company/facebook), the website would pull the individual row for that company from its relational database and display all the relevant information. Unfortunately, if you're performing an aggregation that requires only one column but every row in the dataset, row-oriented databases must still read every column of every row, which can be slow. MySQL, Postgres, and MS SQL Server are examples of row-oriented databases.

* [Column-oriented](http://en.wikipedia.org/wiki/Column-oriented_DBMS): Optimized for aggregations across many rows, column-oriented databases (aka "column-store" or "columnar") read individual columns at a time. If your query only aggregates one column in a 50-column table, a column-oriented database will read in roughly 1/50th the amount of data as a row-oriented database. [Vertica](http://www.vertica.com/hp-vertica-products/the-analytics-platform/) and [Redshift](http://aws.amazon.com/redshift/) are column-oriented databases.

Some database software is capable of storing data on multiple computers &mdash; referred to as a [distributed database](http://en.wikipedia.org/wiki/Distributed_database). When working with a distributed database (like Mode's public database), your queries may actually execute across many of the computers that store data. In other words, your query can be run in pieces on several machines at the same time to speed things up. Optimizing this process is just one way to make your queries run faster. The key to understanding it, and the other secrets of query speed, is becoming familiar with the [query planner](http://en.wikipedia.org/wiki/Query_plan).
-->
###EXPLAIN
Run the following query:

    SELECT *
      FROM 


###Easy Tricks to Speed Things Up
* limit as early as possible in the logic to narrow result set
* 

###Why DISTINCT is so slow

###Indexes
as referenced in [Join Tips & Tricks](/intermediate/join-tips-and-tricks.html)

###Conclusion
Move on to the [last page of the SQL School Tutorial](/advanced/tutorial-conclusion.html).