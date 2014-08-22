---
layout: sqlschool-lesson
category: "intermediate"
title:  "Join Tips and Tricks"
date:   2014-02-01 00:00:51
---

This lesson uses the same data from the previous two lessons, which was pulled from [Crunchbase](http://info.crunchbase.com/about/crunchbase-data-exports/) on Feb. 5, 2014. For more information about the lesson data, read the opening paragraph of the [Outer Joins lesson](/intermediate/outer-joins.html).

###Inequality Joins
So far, you've only joined tables by exactly matching values from both tables. You can enter any type of conditional statement into the `ON` clause, though &mdash; here's an example using `>` to join only investments that occurred more than 5 years after each company's founding year:

    SELECT companies.permalink,
           companies.name,
           companies.status,
           COUNT(investments.investor_permalink) AS investors
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_investments_part1 investments
        ON companies.permalink = investments.company_permalink
       AND investments.funded_year > companies.founded_year + 5
     GROUP BY 1,2, 3

This technique is especially useful for creating date ranges as shown above. It's important to note that this produces a different result than the following query because it only joins rows that fit the `investments.funded_year > companies.founded_year + 5` condition rather than joining all rows and then filtering:

    SELECT companies.permalink,
           companies.name,
           companies.status,
           COUNT(investments.investor_permalink) AS investors
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_investments_part1 investments
        ON companies.permalink = investments.company_permalink
     WHERE investments.funded_year > companies.founded_year + 5
     GROUP BY 1,2, 3

For more on these differences, revisit the "Filtering in the ON clause" section of the [Outer Joins lesson](/intermediate/outer-joins.html).

<!--
<div class="practice-prob">
  Write a query that 
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>
-->

###Joining on multiple keys
There are couple reasons you might want to join on multiple foreign keys. The first has to do with accuracy.

<!--
- explain subcategories
- example

<div class="practice-prob">
  Write a query that 
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>
-->

The second reason has to do with performance. SQL uses "indexes" (essetially pre-defined joins) to speed up queries. This will be covered in greater detail the lesson on [making queries run faster](/advanced/faster-queries.html), but for all you need to know is that it can occasionally make your query run faster to join on multiple fields, even when it does not add to the accuracy of the query. For example, The results of the following query will be the same with or without the last line. However, it is possible to optimize the database such that the query runs more quickly with the last line included:

    SELECT companies.permalink,
           companies.name,
           investments.company_name,
           investments.company_permalink
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_investments_part1 investments
        ON companies.permalink = investments.company_permalink
       AND companies.name = investments.company_name
   
It's worth noting that this will have relatively little effect on small datasets.

###Self-Joins
Sometimes it can be useful to join a table to itself. Let's say you wanted to understand the patterns of investors who invest in the same company multiple times. It would certainly be valuable to look at a list of companies that received repeat investments from the same investor, as well as the names of the investors and the number of repeats (2x, 3x, etc.):

    SELECT investments.company_name,
           investments.company_permalink,
           investments.investor_name,
           COUNT(investments_repeat.investor_name) AS repeat_investments
      FROM tutorial.crunchbase_investments_part1 investments
      JOIN tutorial.crunchbase_investments_part1 investments_repeat
        ON investments.company_permalink = investments_repeat.company_permalink
       AND investments.investor_name = investments_repeat.investor_name
       AND investments.funded_month < investments_repeat.funded_month
     GROUP BY 1,2,3
     ORDER BY 4 DESC

Note that the above query joins on multiple keys to maintain accuracy (we only want to match rows for which the company is the same and the investor is the same). Also, note that the same table can easily be referenced multiple times using different aliases &mdash; in this case, `investments` and `investments_repeat`.

Also, keep in mind as you review the results from the above query that a large part of the data has been omitted for the sake of the lesson (much of it is in the `tutorial.crunchbase_investments_part2` table).

<!--
<div class="practice-prob">
  Write a query that 
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>
-->

<!--
###Joining Multiple Tables
-->

###Next Steps
Congratulations, you've learned most of the technical stuff you need to know to analyze data using SQL. The advanced tutorial covers a few more necessities (in-depth lesson on data types, for example), as well as some more technical features that will greatly extend the tools you've already learned.

<!-- some sort of inspiration, datasets, ways to apply what is learned.-->

Click here to move on to the first advanced lesson: [Data Types](/advanced/data-types.html)