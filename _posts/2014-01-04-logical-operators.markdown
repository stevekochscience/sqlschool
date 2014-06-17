---
layout: sqlschool-lesson
category: "the-basics"
title:  "Using Multiple Filters at Once"
date:   2014-01-01 00:00:56
---

The data in this lesson is from [Billboard Music Charts](http://www.billboard.com/charts). It was collected in January 2014 and contains data from 1956 through 2013. The results in this table are the *year-end* results &mdash; the top 100 songs at the end of each year. Some of the data has been intentionally removed for the sake of this lesson. You can see the full dataset at:

    SELECT * FROM benn.billboard_top_100_year_end

For the lesson, you will use this table. It's very similar:

    SELECT * FROM tutorial.billboard_top_100_year_end

* `year_rank` is the rank of that song at the end of the listed year
* `group_name` is the name of the entire group that won (could be multiple artists if there was a collaboration)
* `artist_name` is an individual artist. This is a little complicated, as an artist can be an individual or group.

You can get a better sense of some of the nuances of this by running the following query. It uses the `ORDER BY` clause, which we will get to in the next lesson. Don't worry about it for now:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     ORDER BY year DESC, year_rank

You'll notice that Macklemore does a lot of collaborations. Since his songs are listed as featuring other artists like Ryan Lewis, there are multiple lines in the dataset for Ryan Lewis. Daft Punk and Pharrell Williams are also listed as two artists. Daft Punk is actually a duo, but since the album lists them together under the name Daft Punk, that's how Billboard treats them.

Now on to the lesson!

###Comparison Operators &mdash; the Weird Ones
In the [previous tutorial](/the-basics/where-operators.html), you played with some comparison operators. There are a couple more that you're likely to find very useful. They're all special snowflakes, so we'll go through them individually:

###LIKE
`LIKE` allows you to match on similar values rather than exact ones. Using wildcard characters, you can define what must be exactly matched and what can be different. In this example, the results will include rows for which `"group"` starts with "Snoop" and is followed by any number and selection of characters.

Note: "group" appears in quotations below because the word "group" is actually the [name of a function in SQL](/intermediate/aggregation-functions.html). The double quotes (as opposed to single: `'`) are a way of indicating that you are referring to the column name "group", not the SQL function. In general, putting double quotes around a word or phrase will always indicate that you are referring to that column name.

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE "group" LIKE 'Snoop%'

The `%` used above represents any character or set of characters. In this case, `%` is referred to as a "wildcard." In the SQL that Mode uses, `LIKE` is case-sensitive, meaning that the above query will only capture matches that start with a capital "S" and lower-case "noop." To match in a way that is not case-sensitive, you can use the `ILIKE` command:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE "group" ILIKE 'snoop%'

You can also use `_` (a single underscore) to substitute for an individual character:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE artist ILIKE 'dr_ke'

<div class="practice-prob">
  Write a query that returns all rows for which Ludacris was a member of the group.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/67228a008c9d" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that returns all rows for which the first artist listed in the group has a name that begins with "DJ"
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/6cc7b85cc096" target="_blank">See the Answer &raquo;</a>
</div>

###IN
You'll probably want to use `IN` pretty frequently &mdash; it allows you to specify a list of values that you'd like to include. For example, the following query will return results for which the `year_rank` column is equal to one of the values in the list:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year_rank IN (1, 2, 3)

As in the previous tutorial, you can use non-numerical values, but they need to go inside single quotes. Regardless of the data type, the values in the list must be separated by commas. Here's another example:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE artist IN ('Taylor Swift', 'Usher', 'Ludacris')

<div class="practice-prob">
  Write a query that shows all of the entries for Elvis and M.C. Hammer. Hint: M.C. Hammer is actually on the list under multiple names, so you may need to first write a query to figure out exactly how M.C. Hammer is listed. You're likely to face similar problems that require some exploration in many real-life scenarios.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/b7c9462034d0" target="_blank">See the Answer &raquo;</a>
</div>

###BETWEEN
This allows you to specify a range and select only rows within a certain range. It has to be paired with the `AND` operator, which you will learn about in just a few moments. Here's how `BETWEEN` looks:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year_rank BETWEEN 5 AND 10

`BETWEEN` includes the values that you specify in the query, so the above query will return the exact same results as the following query:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year_rank >= 5 AND year_rank <= 10

Some people prefer the latter example because it more explicitly shows what the query is doing (t's easy to forget whether or not BETWEEN includes the range bounds).

<div class="practice-prob">
  Write a query that shows all top 100 songs from January 1, 1985 through December 31, 1990.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/4ae007b7d3e3" target="_blank">See the Answer &raquo;</a>
</div>

###IS NULL
Some tables contain null values &mdash; cells with no data in them at all. This can be confusing for heavy Excel users, because the difference between a cell having no data and a cell containing a space isn't meaningful in Excel. In SQL, the implications can be pretty serious. This is covered in greater detail in the [intermediate tutorial](/intermediate/aggregation-functions.html), but for now, here's what you need to know:

You can select rows that contain no data in a given column by using the `IS NULL` operator:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE artist IS NULL

`WHERE artist = NULL` will **not** work &mdash; you can't perform arithmetic on null values.

<div class="practice-prob">
  Write a query that shows all of the rows for which <code>song_name</code> is null.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/c97c1d0b0fba" target="_blank">See the Answer &raquo;</a>
</div>

###Logical Operators
You'll likely want to filter using several conditions &mdash; possibly more often than you filter by only one condition. Logical operators allow you to use multiple comparison operators in one query.

###AND
`AND` will let you select only rows that satisfy two conditions. The following query will return all rows for top-10 recordings in 2012.

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year = 2012 AND year_rank <= 10

You can use `AND` with any comparison operator, and as many times as you want. If you run this query, you'll notice that all of the requirements are satisfied.

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year = 2012
       AND year_rank <= 10
       AND "group" ILIKE '%feat%'

You can see that this example is spaced out onto multiple lines &mdash; a good way to make long `WHERE` clauses more readable.

<div class="practice-prob">
  Write a query that surfaces all rows for top-10 hits for which Ludacris is part of the Group.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/e1731ac0419f" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that surfaces the top-ranked records in 1990, 2000, and 2010
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/5f3ec7055dc0" target="_blank">See the Answer &raquo;</a>
</div>

###OR
If you want to select rows that satisfy either of two conditions, you can use `OR`. It works the same way as `AND`. Try running this:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year_rank = 5 OR artist = 'Gotye'

You'll notice that each row will satisfy one of the two conditions. You can combine `AND` with `OR` using parenthesis. The following query will return rows that satisfy **both** of the following conditions:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year = 2013
       AND ("group" ILIKE '%macklemore%' OR "group" ILIKE '%timberlake%')
   
You will notice that the conditional statement `year = 2013` will be fulfilled for every row returned. `(or)` is treated like one separate conditional statement because it is in parentheses, so it must be satisfied in addition to the first statement of `year = 2013`. You can think of the rows selected as being either of the following:

* Rows where `year = 2013` is true and `group_name ILIKE '%macklemore%'` is true
* Rows where `year = 2013` is true and `group_name ILIKE '%timberlake%'` is true
* Rows where `year = 2013` is true and `group_name ILIKE '%macklemore%'` is true and `group_name ILIKE '%timberlake%'` is true

<div class="practice-prob">
  Write a query that returns all rows for top-10 songs that featured either Katy Perry or Bon Jovi
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/aa5a19da7f2e" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that returns all songs with titles that contain the word "California" in either the 1970s or 1990s.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/ca79c861050c" target="_blank">See the Answer &raquo;</a>
</div>
   
###NOT
You can add `NOT` before any conditional statement if you'd like to select the rows for which that statement is false.

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year = 2013 
       AND year_rank NOT 1

Using `NOT` with `<` and `>` usually doesn't make sense because you can simply use the opposite comparative operator instead. `NOT` is more commonly used with `LIKE`. Run this query and check out how Macklemore magically disappears!

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year = 2013 
       AND "group" NOT ILIKE '%macklemore%'

`NOT` is mostly commonly used to identify non-null rows, but the syntax is somewhat special &mdash; you need to include `IS` beforehand. Here's how that looks:

    SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE year = 2013 
       AND artist IS NOT NULL

<div class="practice-prob">
  Write a query that returns all rows for songs that were on the charts in 2013 and do not contain the letter "a"
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/f64867a56523" target="_blank">See the Answer &raquo;</a>
</div>

###Practice Problems

<div class="practice-prob">
  Write a query that lists all top-100 recordings feature Dr. Dre before 2001 or after 2009.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/88c81c1a02e8" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that lists all songs from the 1960s with "love" in the title.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/11c78511873a" target="_blank">See the Answer &raquo;</a>
</div>

Move on to the next segment: [Ordering Your Results](/the-basics/order-by.html).