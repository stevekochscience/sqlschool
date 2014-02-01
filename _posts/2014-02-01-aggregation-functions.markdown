---
layout: sqlschool-lesson
category: "intermediate"
title:  "Aggregation Functions"
date:   2014-02-01 00:00:59
---

<!--apple stock data-->


<div class="tip">Tip: Start by running this code to get a sense of what's in the table that will be used in this tutorial.</div>

    SELECT * FROM tutorial.sat_scores

###Aggregation Functions
As the [beginner tutorial](/the-basics/basic-concepts.html) points out, SQL is excellent at aggregating data the way you might in a pivot table in Excel. You will use aggregation functions all the time, so it's important to get comfortable with them. The functions themselves are the same ones you will find in Excel or any other analytics program: `COUNT`, `SUM`, `MIN`, `MAX`, `AVG`

###COUNT
It's easiest to start with `COUNT` because verifying your results is extremely simply. The easiest way to get started is to use `*` to select all rows.

    SELECT COUNT(*)
      FROM tutorial.sat_scores

You can see that 44 results were returned. Run the following query and note that Mode actually provides a row count, which should be the same as the result of the above query:

    SELECT *
      FROM tutorial.sat_scores

Things start to get a little bit tricky when you want to count individual columns. The following code will provide a count of all of rows in which columnb **is not null**.

    SELECT COUNT(student_id)
      FROM tutorial.sat_scores

You'll notice that this result is lower than what you got with `COUNT(*)`. That's because columnb has some nulls.

One nice thing about `COUNT` is that you can use it on non-numerical columns:

    SELECT COUNT(teacher)
      FROM tutorial.sat_scores

The above query returns the same result as the previous &mdash; 44 &mdash; even when there are only 3 different teacher names in the table. It's important to keep in mind that `COUNT` simply counts the total number of non-null rows, not the distinct values, which are discussed in a [later tutorial](LINK) 

You might have also noticed that the column header in the results just reads "count." We recommend naming your columns so that they make a little more sense to the reader. As mentioned in the [previous tutorial](LINK), it's best to use lower case letters and underscores. You can add column names (also called *aliases*) using `AS`:

    SELECT COUNT(student_id) AS student_count
      FROM tutorial.sat_scores

If you must use spaces, you will need to use double quotes.

    SELECT COUNT(student_id) AS "Student Count"
      FROM tutorial.sat_scores

*Note: This is really the only place in which you'll ever want to use double quotes in SQLl; single quotes for everything else.*

###SUM
This works the same way as count, except that it can only be used on numerical columns. As you might expect, `SUM` totals all the values in a given column:

    SELECT SUM(hrs_studied)
      FROM tutorial.sat_scores

An important thing to remember: aggregators only aggregate vertically. In this data, you might want to aggregate across a row to add all 3 of a student's scores to get a total score. You would do this with [simple arithmetic](LINK TO EARLIER TUTORIAL):

    SELECT sat_math,
           sat_verbal,
           sat_writing,
           sat_math + sat_verbal + sat_writing AS sat_total
      FROM tutorial.sat_scores

You don't need to worry as much about the presence of nulls with `SUM` as you would with `COUNT` &mdash; `SUM` treats nulls as 0.

###MIN/MAX
`MIN` and `MAX` are similar to `COUNT` in that they can be used on non-numerical columns. Depending on the column type, `MIN` will return the lowest number, earliest date, or non-numerical value as close to "a" as possible. As you might suspect, `MAX` does the opposite:

    SELECT MIN(hrs_studied) AS min_study_time,
           MAX(hrs_studied) AS max_study_time
      FROM tutorial.sat_scores

Nulls are treated as lower than 0 or "a" so `MIN` will return a null value if the column contains one. If you want the lowest real number or value, you can filter out nulls using a `WHERE` clause:

    SELECT MIN(hrs_studied) AS min_study_time,
           MAX(hrs_studied) AS max_study_time
      FROM tutorial.sat_scores
     WHERE hrs_studied IS NOT NULL

###AVG
`AVG` does what you'd think &mdash; it calculated the average of selected values. It is very useful, but has some limitations. First, it can only be used on numerical columns. Second, it ignores nulls completely. There are some cases in which you will want to treat null values as 0. For these cases, you'll want to write a statement that changes the nulls to 0 (covered in the [intermediate tutorial](LINK TO CASE STATEMENT)).

You can see this by comparing these two queries:

    SELECT AVG(hrs_studied)
      FROM tutorial.sat_scores
     WHERE hrs_studied IS NOT NULL
     
Produces the same result as:

    SELECT AVG(hrs_studied)
      FROM tutorial.sat_scores

###GROUP BY
All of the queries above have something in common: they all aggregate across the entire table. What if you want to aggregate only part of the table &mdash; you want to count entries by month, for example. `GROUP BY` allows you to separate your aggregations into sub-groups. Here's an example:

    SELECT teacher,
           COUNT(student_id)
      FROM tutorial.sat_scores
     GROUP BY teacher

You can group by multiple columns, but you have to separate column names with commas &mdash; just as with `ORDER BY`:

    SELECT school,
           teacher,
           COUNT(student_id)
      FROM tutorial.sat_scores
     GROUP BY school, teacher

As in [ORDER BY](LINK), you can substitute numbers for column names in the `GROUP BY` clause. It's generally recommended to do this only when you are grouping many columns, or if something else is causing the text in the `GROUP BY` clause to be excessively long:

    SELECT school,
           teacher,
           COUNT(student_id)
      FROM tutorial.sat_scores
     GROUP BY 1, 2

*Note: this functionality (numbering columns instead of using names) is supported by Mode, but not by every flavor of SQL, so if you're using another system or connected to certain types of databases, it may not work.*

The order of column names in your `GROUP BY` clause doesn't matter &mdash; the results will be the same regardless. If you want to control how the aggregations are grouped together, use `ORDER BY`. Try running the query below, then reverse the column names in the `ORDER BY` statement and see how it looks.

    SELECT school,
           teacher,
           COUNT(student_id)
      FROM tutorial.sat_scores
     GROUP BY 1, 2
     ORDER BY 1, 2

There's one thing to be aware of as you group by multiple columns: SQL evaluates the aggregations before the `LIMIT` clause. If you don't group by any columns, you'll get a 1-row result &mdash; no problem there. If you group by a column with enough unique values that it exceeds the `LIMIT` number, what will happen is that aggregates will be calculated, and then some rows will simply be omitted from the results. This is actually a nice way to do things because you know you're going to get the correct aggregates. If SQL cut the table down to 100 rows, then performed the aggregations, your results would be substantially different.

###Query Clause Order
As mentioned in prior tutorials, the order in which you write the clauses is important. Here's the order for everything you've learned so far:

1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. ORDER BY

###Aggregation Practice
<!-- practice problems-->


Check out the next lesson: [DISTINCT](/intermediate/distinct.html)
