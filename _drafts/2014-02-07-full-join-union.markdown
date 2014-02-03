---
layout: sqlschool-lesson
category: "intermediate"
title:  "Full Joins and UNION"
date:   2014-02-01 00:00:53
---

###FULL OUTER JOIN
You're not likely to use `FULL JOIN` (can also be written as `FULL OUTER JOIN`) too often, but it's worth covering anyway. `LEFT JOIN` and `RIGHT JOIN` each return unmatched rows from one of the tables &mdash; `FULL JOIN` returns unmatched rows from both tables. It is commonly used in conjunction with aggregations to understand the amount of overlap between two tables:

    SELECT COUNT(CASE WHEN __ IS NULL THEN 1 ELSE NULL END) AS table1_count,
           COUNT(CASE WHEN __ IS NULL THEN 1 ELSE NULL END) AS table2_count,
           COUNT(CASE WHEN __ IS NOT NULL AND __ IS NOT NULL THEN 1 ELSE NULL END) AS overlap_count
    FROM table

###UNION
Joins allow you to combine two datasets side-by-side, but `UNION` allows you to stack one dataset on top of the other? Put differently, `UNION` allows you to append everything in a dataset onto everything in another dataset. In order to specify the datasets you want to sandwich together, you'll need to actually write two separate `SELECT` statements:

<!-- UNION EXAMPLE -->

Note that `UNION` only appends distinct values. If you'd like to append all the values from the second table, use `UNION ALL`. You'll likely use `UNION ALL` far more often than `UNION`.

<!-- UNION ALL example -->

SQL has strict rules for appending data:

1. Both tables must have the same number of columns
2. The columns must have the same data types in the same order as the first table

While the column names don't necessarily have to be the same, you will find that they typically are. This is because most of the instances in which you'd want to use `UNION` involve stitching together different parts of the same dataset. For example, if you've got several sets of stock ticker data &mdash; one table for each ticker symbol &mdash; you might want to `UNION` the tables so that you can aggregate across multiple ticker symbols:

<!-- ticker symbol example -->

Keep in mind that since you are writing two separate `SELECT` statements, you can filter them differently using different `WHERE` clauses:

    SELECT * FROM ____ WHERE x
     UNION ALL
    SELECT * FROM ____ WHERE y

###Practice Time
<!-- full join -->
<!-- union all example-->

###Next Steps
Congratulations, you've learned most of the technical stuff you need to know to get around Mode. The advanced tutorial covers some more technical features that will greatly extend the tools you've already learned, as well as some tips to manage different types of data and make your queries run faster.

Click here to move on to the next tutorial: [Data Types](/advanced/data-types.html)