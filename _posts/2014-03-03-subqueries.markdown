---
layout: sqlschool-lesson
category: "advanced"
title:  "Subqueries"
date:   2014-03-01 00:00:57
---

In this lesson, you will continue to work with the same [San Francisco Crime data](https://data.sfgov.org/Public-Safety/Map-Crime-Incidents-Previous-Three-Months/gxxq-x39z) used in the [previous lesson](/advanced/data-cleaning.html).

###Subquery basics
Subqueries (also known as inner queries or nested queries) are a tool for performing operations in multiple steps. For example, if you wanted to take the sums of several columns, then average all of those values, you'd need to do each aggregation in a distinct step.

Subqueries can be used in several places within a query, but it's easiest to start with the `FROM` statement. Here's an example of a basic subquery:

    SELECT sub.*
      FROM (
            SELECT *
              FROM tutorial.sf_crime_incidents_2014_01
             WHERE day_of_week = 'Friday'
           ) sub
     WHERE sub.resolution = 'NONE'

Let's break down what happens when you run the above query:

First, the database runs the "inner query" &mdash; the part between the parentheses:

    SELECT *
      FROM tutorial.sf_crime_incidents_2014_01
     WHERE day_of_week = 'Friday'

If you were to run this on its own, it would produce a result set like any other query. It might sound like a no-brainer, but it's important: your inner query must actually run on its own, as the database will treat it as an independent query. Once the inner query runs, the outer query will run *using the results from the inner query as its underlying table*:

    SELECT sub.*
      FROM (
           <<results from inner query go here>>
           ) sub
     WHERE sub.resolution = 'NONE'

Subqueries are required to have names, which are added after parentheses [the same way you would add an alias to a normal table](/intermediate/inner-joins.html). In this case, we've used the name "sub." 

A quit note on formatting: The important thing to remember when using subqueries is to provide some way to for the reader to easily determine which parts of the query will be executed together. Most people do this by indenting the subquery in some way. The examples in this tutorial are indented quite far &mdash; all the way to the parentheses. This isn't practical if you nest many subqueries, so it's fairly common to only indent two spaces or so.

<div class="practice-prob">
  Write a query that selects all Warrant Arrests from the <code>tutorial.sf_crime_incidents_2014_01</code> dataset, when wraps it in an outer query that filters out unresolved incidents.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/3af88ef4ee7a" target="_blank">See the Answer &raquo;</a>
</div>

The above examples, as well as the practice problem don't really require subqueries &mdash; they solve problems that could also be solved by adding multiple conditions to the `WHERE` clause. These next sections provide examples for which subqueries are the best or only way to solve their respective problems.

###Using Subqueries to aggregate in multiple stages
What if you wanted to figure out how many indicents get reported on each day of the week? Better yet, what if you wanted to know how many incidenct happen, on average, on a Friday in December? In January? There are two steps to this process: counting the number of incidents each day (inner query), then determining the monthly average (outer query): 

    SELECT LEFT(sub.date, 2) AS cleaned_month,
           sub.day_of_week,
           AVG(sub.incidents) AS average_incidents
      FROM (
            SELECT day_of_week,
                   date,
                   COUNT(incidnt_num) AS incidents
              FROM tutorial.sf_crime_incidents_2014_01
             GROUP BY 1,2
           ) sub
     GROUP BY 1,2
     ORDER BY 1,2

If you're having trouble figuring out what's happening, try running the inner query individually to get a sense of what its results look like. In general, it's easiest to write inner queries first and revise them until the results make sense to you, then to move on to the outer query.

<div class="practice-prob">
  Write a query that displays the average number of incidents per month, by category. Hint: use <code>tutorial.sf_crime_incidents_cleandate</code> to make your life a little easier.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/7a6b1f866394" target="_blank">See the Answer &raquo;</a>
</div>

###Subqueries in conditional logic
You can use subqueries in conditional logic (in conjunction with `WHERE`, `JOIN`/`ON`, or `CASE`). The following query returns all of the entries from the earliest date in the dataset (theoretically &mdash; the poor formatting of the date column actually makes it return the value that sorts first alphabetically):

    SELECT *
      FROM tutorial.sf_crime_incidents_2014_01
     WHERE Date = (SELECT MIN(date)
                     FROM tutorial.sf_crime_incidents_2014_01
                  )

The above query works because the result of the subquery is only one cell. Most conditional logic will work with subqueries containing one-cell results. However, `IN` is the only type of conditional logic that will work when the inner query contains multiple results:

    SELECT *
      FROM tutorial.sf_crime_incidents_2014_01
     WHERE Date IN (SELECT date
                     FROM tutorial.sf_crime_incidents_2014_01
                    ORDER BY date
                    LIMIT 5
                  )

Note that you should not include an alias when you write a subquery in a conditional statement. This is because the subquery is treated as an individual value (or set of values in the `IN` case) rather than as a table.

<!--
<div class="practice-prob">
  Practice Problem
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>
-->

###Joining Subqueries
You may remember from the [Outer Join lesson](/intermediate/outer-joins.html) that you can filter queries in joins. It's fairly common to join a subquery that hits the same table as the outer query rather than filtering in the `WHERE` clause. The following query produces the same results as the previous example:

    SELECT *
      FROM tutorial.sf_crime_incidents_2014_01 incidents
      JOIN ( SELECT date
               FROM tutorial.sf_crime_incidents_2014_01
              ORDER BY date
              LIMIT 5
           ) sub
        ON incidents.date = sub.date

This can be particularly useful when combined with aggregations. When you join, the requirements for your subquery output aren't as stringent as when you use the `WHERE` clause. For example, your inner query can output multiple results. The following query ranks all of the results according to how many incidents were reported in a given day. It does this by ranking the total number of incidents each day in the inner query, then using those values to sort the outer query:

    SELECT incidents.*,
           sub.incidents AS incidents_that_day
      FROM tutorial.sf_crime_incidents_2014_01 incidents
      JOIN ( SELECT date,
              COUNT(incidnt_num) AS incidents
               FROM tutorial.sf_crime_incidents_2014_01
              GROUP BY 1
           ) sub
        ON incidents.date = sub.date
     ORDER BY sub.incidents DESC, time

<div class="practice-prob">
  Write a query that displays all rows from the three categories with the fewest incidents reported.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/1a6ee6229859" target="_blank">See the Answer &raquo;</a>
</div>

Subqueries can be very helpful in improving the performance of your queries. Let's revisit the [Crunchbase Data](/intermediate/outer-joins.html) briefly. Imagine you'd like to aggregate all of the copmanies receiving investment and companies acquired each month. You could do that without subqueries if you wanted to, but **don't actually run this as it will take minutes to return**:

    SELECT COALESCE(acquisitions.acquired_month, investments.funded_month) AS month,
           COUNT(DISTINCT acquisitions.company_permalink) AS companies_acquired,
           COUNT(DISTINCT investments.company_permalink) AS
      FROM tutorial.crunchbase_acquisitions acquisitions
      FULL JOIN tutorial.crunchbase_investments investments
        ON acquisitions.acquired_month = investments.funded_month
     GROUP BY 1

Note that in order to do this properly, you must join on date fields, which causes a massive "data explosion." Basically, what happens is that you're joining every row in a given month from one table onto every month in a given row on the other table, so the number of rows returned is incredibly great. Because of this multiplicative effect, you must use `COUNT(DISTINCT)` instead of `COUNT` to get accurate counts. You can see this below:

The following query shows 7,414 rows:

    SELECT COUNT(*) FROM tutorial.crunchbase_acquisitions

The following query shows 83,893 rows:

    SELECT COUNT(*) FROM tutorial.crunchbase_investments

The following query shows 6,237,396 rows:

        SELECT COUNT(*)
          FROM crunchbase_acquisitions acquisitions
          FULL JOIN crunchbase_investments investments
            ON acquisitions.acquired_month = investments.funded_month

If you'd like to understand this a little better, you can do some extra research on [cartesian products](http://en.wikipedia.org/wiki/Cartesian_product). It's also worth noting that the `FULL JOIN` and `COUNT` above actually runs pretty fast &mdash; it's the `COUNT(DISTINCT)` that takes forever. More on that in the [lesson on optimizine queries](/advanced/faster-queries.html)

Of course, you could solve this much more efficiently by aggregating the two tables separately, then joining them together so that the counts are performed across far smaller datasets:

    SELECT COALESCE(acquisitions.month, investments.month) AS month,
           acquisitions.companies_acquired,
           investments.companies_rec_investment
      FROM (
            SELECT acquired_month AS month,
                   COUNT(DISTINCT company_permalink) AS companies_acquired
              FROM tutorial.crunchbase_acquisitions
             GROUP BY 1
           ) acquisitions
      
      FULL JOIN (
            SELECT funded_month AS month,
                   COUNT(DISTINCT company_permalink) AS companies_rec_investment
              FROM tutorial.crunchbase_investments
             GROUP BY 1
           )investments
        
        ON acquisitions.month = investments.month
     ORDER BY 1 DESC

Note: We used a `FULL JOIN` above just in case one table had observations in a month that the other table didn't. We also used `COALESCE` to display months when the `acquisitions` subquery didn't have month entries (presumably no acquisitions occurred in those months). We strongly encourage you to re-run the query without some of these elements to better understand how they work. You can also run each of the subqueries independently to get a better understanding of them as well.

<div class="practice-prob">
  Write a query that counts the number of companies founded and acquired by quarter starting in Q1 2012. Create the aggregations in two separate queries, then join them.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/6ebd90cda35c" target="_blank">See the Answer &raquo;</a>
</div>

###Subqueries and UNIONs
For this next section, we will borrow directly from the lesson on [UNIONs](/intermediate/full-join-union/html) &mdash; again using the Crunchbase data:

    SELECT *
      FROM tutorial.crunchbase_investments_part1
     
     UNION ALL
    
     SELECT *
       FROM tutorial.crunchbase_investments_part2

It's certainly not uncommon for a dataset to come split into several parts, especially if the data passed through Excel at any point (Excel can only handle ~1M rows per spreadsheet). The two tables used above can be thought of as different parts of the same dataset &mdash; what you'd almost certainly like to do it perform operations on the entire combined dataset rather than on the individual parts. You can do this by using a subquery:

    SELECT COUNT(*) AS total_rows
      FROM (
            SELECT *
              FROM tutorial.crunchbase_investments_part1
             
             UNION ALL
            
            SELECT *
              FROM tutorial.crunchbase_investments_part2
           ) sub

This is pretty straight-forward. Try it for yourself:

<div class="practice-prob">
  Write a query that ranks investors from the combined dataset above by the total number of investments they have made.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/740b0e583576" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that does the same thing as in the previous problem, except only for companies that are still operating. Hint: operating status is in <code>tutorial.crunchbase_companies</code>.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/019819a0608d" target="_blank">See the Answer &raquo;</a>
</div>

Move on to the next lesson: [Window Functions](/advanced/window-functions.html).