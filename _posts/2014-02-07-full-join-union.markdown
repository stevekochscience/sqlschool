---
layout: sqlschool-lesson
category: "intermediate"
title:  "Full Joins and Appending Data"
date:   2014-02-01 00:00:53
---
<!-- This introduction about using tutorial.crunchbase_investments was already given on the bottom of the page we just finished.  Are you sure you want it here again in such similar phrasing?  At the very least, you should remove "This time, however,".

Also, reading a bit ahead you reference tutorial.crunchbase_investments in this intro paragraph when in actuality we will be using tutorial.crunchbase_investments_part1 in this tutorial.  If it's different data from the last lesson, why not say so from the beginning?  -->

This lesson uses the same data from the previous lesson, which was pulled from [Crunchbase](http://info.crunchbase.com/about/crunchbase-data-exports/) on Feb. 5, 2014. This time, however, you will also use the `tutorial.crunchbase_investments` table, which contains one row per investor, per investment. So a particular investment in a company on a given date might show up in multiple rows if multiple investors participated. As in the previous lesson, large portions of both tables were randomly dropped for the sake of this lesson. For more complete notes on Crunchbase data, return to the [previous lesson](/intermediate/outer-joins.html). For now, you should acquaint yourself with the `tutorial.crunchbase_investments` table:

    SELECT *
      FROM tutorial.crunchbase_investments_part1

###FULL OUTER JOIN
You're not likely to use `FULL JOIN` (can also be written as `FULL OUTER JOIN`) too often, but it's worth covering anyway. `LEFT JOIN` and `RIGHT JOIN` each return unmatched rows from one of the tables &mdash; `FULL JOIN` returns unmatched rows from both tables. It is commonly used in conjunction with aggregations to understand the amount of overlap between two tables. Here's an example using the Crunchbase companies and acquisitions tables:

    SELECT COUNT(CASE WHEN companies.permalink IS NOT NULL AND acquisitions.company_permalink IS NULL
                      THEN companies.permalink ELSE NULL END) AS companies_only,
           COUNT(CASE WHEN companies.permalink IS NOT NULL AND acquisitions.company_permalink IS NOT NULL
                      THEN companies.permalink ELSE NULL END) AS both_tables,
           COUNT(CASE WHEN companies.permalink IS NULL AND acquisitions.company_permalink IS NOT NULL
                      THEN acquisitions.company_permalink ELSE NULL END) AS acquisitions_only
      FROM tutorial.crunchbase_companies companies
      FULL JOIN tutorial.crunchbase_acquisitions acquisitions
        ON companies.permalink = acquisitions.company_permalink

One important thing to keep in mind is that you must count from the `crunchbase_acquisitions` table in order to get unmatched rows in that table &mdash; if you were to count `companies.permalink` as in the first two columns, you would get a result of 0 in the third column because it would be counting up a bunch of null values.

You might also notice that surprisingly few rows in the `crunchbase_acquisitions` table were matched to the `crunchbase_companies` table. If this were a real assignment, you'd probably want to look at some individual rows to get a sense of why some of them weren't matched and whether or not you should consider finding more/better data.

<!-- The way I read this problem I ended up with a query that gave me a 'matched' and an 'unmatched' column.  The way it stands now this practice problem is either confusingly worded or just requiring us to copy/paste the example above and change one table name, or maybe both.  -->

<div class="practice-prob">
  Write a query that joins <code>tutorial.crunchbase_companies</code> and <code>tutorial.crunchbase_investments_part1</code> using a <code>FULL JOIN</code>. Count up the number of rows that are matched/unmatched as in the example above.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/524108a9e0b0" target="_blank">See the Answer &raquo;</a>
</div>

###UNION

<!-- This doesn't do a good job of explaining what UNION is actually doing.  I've read it a few times and I still have no idea. -->

Joins allow you to combine two datasets side-by-side, but `UNION` allows you to stack one dataset on top of the other. Put differently, `UNION` allows you to append everything in a dataset onto everything in another dataset. In order to specify the datasets you want to sandwich together, you'll need to actually write two separate `SELECT` statements:

    SELECT *
      FROM tutorial.crunchbase_investments_part1
     
     UNION
    
     SELECT *
       FROM tutorial.crunchbase_investments_part2

Note that `UNION` only appends distinct values. More specifically, when you use `UNION`, the dataset is appended, and any rows in the appended table that are exactly identical to rows in the first table are dropped. If you'd like to append all the values from the second table, use `UNION ALL`. You'll likely use `UNION ALL` far more often than `UNION`. In this particular case, there are no duplicate rows, so `UNION ALL` will produce the same results:

    SELECT *
      FROM tutorial.crunchbase_investments_part1
     
     UNION ALL
    
     SELECT *
       FROM tutorial.crunchbase_investments_part2

SQL has strict rules for appending data:

1. Both tables must have the same number of columns
2. The columns must have the same data types in the same order as the first table

While the column names don't necessarily have to be the same, you will find that they typically are. This is because most of the instances in which you'd want to use `UNION` involve stitching together different parts of the same dataset (as is the case here).

Since you are writing two separate `SELECT` statements, you can treat them differently before appending. For example, you can filter them differently using different `WHERE` clauses.

<div class="practice-prob">
  Write a query that appends the two crunchbase_investments datasets above (including duplicate values). Filter the first dataset to only companies with names that start with the letter "T", and filter the second to companies names starting with "M" (both not case-sensitive). Only include the <code>company_permalink</code>, <code>company_name</code>, and <code>investor_name</code> columns.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/46ac1e3a5886" target="_blank">See the Answer &raquo;</a>
</div>

For a bit more of a challenge:

<!-- Take LIMIT out of the answer -->

<div class="practice-prob">
  Write a query that shows 3 columns. The first indicates which dataset (part 1 or 2) the data comes from, the second shows company status, and the third is a count of the number of investors. Hint: you will have to use the <code>tutorial.crunchbase_companies</code> table as well as the investments tables.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/e8ebd7cc9d23" target="_blank">See the Answer &raquo;</a>
</div>

<!-- This links back to the top of this page.  Should go back to index or wherever you want to send people finished with intermediate level. -->

Check out the next lesson: [Join Tips and Tricks](/intermediate/full-join-union.html)