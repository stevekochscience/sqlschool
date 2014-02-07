---
layout: sqlschool-lesson
category: "intermediate"
title:  "Outer Joins"
date:   2014-02-01 00:00:54
---

The data for this lesson was pulled from [Crunchbase](http://info.crunchbase.com/about/crunchbase-data-exports/), a crowdsourced index of startups, founders, investors, and the activities of all three. It was collected Feb. 5, 2014, and large portions of both tables were randomly dropped for the sake of this lesson. The first table lists a large portion of companies in the database; one row per company. The `permalink` field is a unique identifier for each row, and also shows the web address. For the 3rd company in the table, ".Club Domains," you can view its online Crunchbase profile by copy/pasting its permalink, "/company/club-domains" after Crunchbase's web domain: [http://www.crunchbase.com/company/club-domains](http://www.crunchbase.com/company/club-domains). The fields with "funding" in the name have to do with how much outside investment (in USD) each company has taken on. The rest of the fields are self-explanatory.

    SELECT *
      FROM tutorial.crunchbase_companies

The second table lists acquisitions &mdash; one row per acquisition. `company_permalink` in this table is what is called a "join key." It will allow you to append data about acquired companies onto all of the data about each company in `tutorial.crunchbase_companies`. You'll notice that there is a separate field called `acquirer_permalink` as well. This can also be mapped to `tutorial.crunchbase_companies` to get additional information about the acquiring company (if available).

    SELECT *
      FROM tutorial.crunchbase_acquisitions

###Outer Joins
When performing an Inner Join, rows from either table that are unmatched in the other table are not returned. In an Outer Join, unmatched rows in one or both tables can be returned. There are a few types of outer joins. `LEFT JOIN` and `RIGHT JOIN` only return unmatched rows from one table, while `FULL OUTER JOIN` returns unmatched rows from both tables.

###LEFT JOIN
![Left Joins](http://www.w3schools.com/sql/img_leftjoin.gif)

Let's start by running an `INNER JOIN` and taking a look at the results. We'll just look at `company-permalink` in each table, as well as a couple other fields, to get a sense of what's actually being joined.

    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisitions_permalink,
           acquisitions.acquired_at AS acquired_date
      FROM tutorial.crunchbase_companies companies
      JOIN tutorial.crunchbase_acquisitions acquisitions
        ON companies.permalink = acquisitions.company_permalink

You may notice that "280 North" appears twice in this list. That is because is has two entried in the `tutorial.crunchbase_acquisitions` table, both of which are being joined onto the `tutorial.crunchbase_companies` table.

![Inner Join Results](/images/intermediate/inner-join-results.png)

Now try running that query as a `LEFT JOIN`:

    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisitions_permalink,
           acquisitions.acquired_at AS acquired_date
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
        ON companies.permalink = acquisitions.company_permalink

You can see that the first two companies from the previous result set, #waywire and 1000memories, are pushed down the page by a number of results that contain null values in the `acquisitions_permalink` and `acquired_date` fields.

![Left Join Results](/images/intermediate/left-join-results.png)

This is because the `LEFT JOIN` command tells the database to return all rows in the table in the `FROM` clause, regardless of whether or not they have matches in the table in the `LEFT JOIN` clause. You can explore the differences between a `LEFT JOIN` and a `JOIN` by solving these practice problems:

<div class="practice-prob">
  Write a query that performs an Inner Join between the `tutorial.crunchbase_acquisitions` table and the `tutorial.crunchbase_companies` table as in the above examples, but instead of listing individual rows, count the number of non-null rows in each table.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/e6cde36b3e4a" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Modify the query above to be a `LEFT JOIN`. Note the difference in results.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/0653d8834126" target="_blank">See the Answer &raquo;</a>
</div>

Now that you've got a sense of how Left Joins work, try this harder aggregation problem:

<div class="practice-prob">
  Count the number of unique companies (don't double-count companies) and unique *acquired* companies by state. Do not include results for which there is no state data, and order by the number of acquired companies from highest to lowest.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/8805606278ee" target="_blank">See the Answer &raquo;</a>
</div>

###RIGHT JOIN
Right Joins are similar to Left Joins except they return all rows from the table in the `RIGHT JOIN` clause and only matching rows from the table in the `FROM` clause.

![Right Joins}(http://www.w3schools.com/sql/img_rightjoin.gif)

`RIGHT JOIN` is rarely used because you can achieve the results of a Right Join by simply switching the two joined table names in a Left Join. For example, this query produces the same results as the previous query:

    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisitions_permalink,
           acquisitions.acquired_at AS acquired_date
      FROM tutorial.crunchbase_acquisitions acquisitions
     RIGHT JOIN tutorial.crunchbase_companies companies
        ON companies.permalink = acquisitions.company_permalink

The convention of always using `LEFT JOIN` probably exists to make queries easier to read/audit, but beyond that there isn't necessarily a strong reason to avoid using Right Join.

<div class="practice-prob">
  Rewrite the previous practice query in which you counted total and acquired companies by state, but with a `RIGHT JOIN` instead of a `LEFT JOIN`. The goal is to produce the exact same results.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/f84c80c14c7a" target="_blank">See the Answer &raquo;</a>
</div>

It's worth noting that `LEFT JOIN` and `RIGHT JOIN` can be written as `LEFT OUTER JOIN` and `RIGHT OUTER JOIN`, respectively.

###Filtering in the ON clause
Let's go back to the `LEFT JOIN` example (this time we will add an `ORDER BY` clause):

    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisitions_permalink,
           acquisitions.acquired_at AS acquired_date
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
        ON companies.permalink = acquisitions.company_permalink
     ORDER BY 1

Normally, filtering is processed in the `WHERE` clause once the two tables have already been joined. It's possible, though that you might want to filter one or both of the tables *before* joining them. For example, you only want to create matches between the tables under certain circumstances. Compare the following query to the previous one and you will see that everything in the `tutorial.crunchbase_acquisitions` table was joined on **except** for the row for which `company_permalink` is `'/company/1000memories'`:

    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisitions_permalink,
           acquisitions.acquired_at AS acquired_date
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
        ON companies.permalink = acquisitions.company_permalink
       AND acquisitions.company_permalink != '/company/1000memories'
     ORDER BY 1

What's happening above is that the conditional statement `AND...` is evaluated before the join occurs. You can think of it as a where clause that only applies to one of the tables. You can tell tha this is only happening in one of the tables because the 1000memories permalink is still displayed in the column that pulls from the other table:

![Left Join ON Clause Results](/images/intermediate/left-join-on-clause-results.png)

If you move the same filter to the `WHERE` clause, you will notice that the filter happens after the tables are joined. The result is that the 1000memories row is joined onto the original table, but then it is filtered out entirely (in both tables) in the `WHERE` clause before displaying results.

    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisitions_permalink,
           acquisitions.acquired_at AS acquired_date
      FROM tutorial.crunchbase_companies companies
      LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
        ON companies.permalink = acquisitions.company_permalink
     WHERE acquisitions.company_permalink != '/company/1000memories'
        OR acquisitions.company_permalink IS NULL
     ORDER BY 1

You can see that the 1000memories line is not returned (it would have been between the two highlighted lines below). Also note that filtering in the `WHERE` clause can also filter null values, so we added an extra line to make sure to include the nulls.

![Left Join ON Clause Results](/images/intermediate/left-join-on-clause-results.png)

###Practice Problems
For this set of practice problems, we're going to introduce a new dataset: `tutorial.crunchbase_investments`. This table is also sourced from Crunchbase and contains much of the same information as the `tutorial.crunchbase_companies` data. It it structured differently, though: it contains one row per *investment*. There can be multiple investments per company &mdash; it's even possible that one investor could invest in the same company multiple times. The column names are pretty self-explanatory. What's important is that `company-permalink` in the `tutorial.crunchbase_investments` table maps to `permalink` in the `tutorial.crunchbase_companies` table. Keep in mind that some random data has been removed from this table for the sake of this lesson.

It is very likely that you will need to do some exploratory analysis on this table to understand how you might solve the following problems.

<div class="practice-prob">
  Write a query that shows a company's name, "status" (found in the Companies table), and the number of unique investors in that company. Order by the number of investors from most to fewest. Limit to only companies in New York.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/1cf1d38ba1fc" target="_blank">See the Answer &raquo;</a>
</div>

<!--><div class="practice-prob">
  practice
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Outer Joins practice prob 1
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>
-->
Check out the next lesson: [Full Join and UNION](/intermediate/full-join-union.html)