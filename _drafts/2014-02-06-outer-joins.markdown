---
layout: sqlschool-lesson
category: "intermediate"
title:  "Outer Joins"
date:   2014-02-01 00:00:54
---

The data for this lesson was pulled from [Crunchbase](http://info.crunchbase.com/about/crunchbase-data-exports/), a crowdsourced index of startups, founders, investors, and the activities of all three. It was collected Feb. 5, 2014, and large portions of both tables were randomly dropped for the sake of this lesson. The first table shows...:

    SELECT *
      FROM tutorial.crunchbase_companies

The second table shows...:

    SELECT *
      FROM tutorial.crunchbase_acquisitions

###Outer Joins
When performing an Inner Join, rows from either table that are unmatched in the other table are not returned. In an Outer Join, unmatched rows in one or both tables can be returned. There are a few types of outer joins. `LEFT JOIN` and `RIGHT JOIN` only return unmatched rows from one table, while `FULL OUTER JOIN` returns unmatched rows from both tables.

###LEFT JOIN
![Left Joins](http://www.w3schools.com/sql/img_leftjoin.gif)

Let's start by looking at the previous example of an INNER JOIN:

    SELECT users.*,
           tweets.*
      FROM tutorial.tweets tweets
      JOIN tutorial.twitter_users users
        ON users.user_id = tweets.tweeted_by_id

Now try running it as a `LEFT JOIN` instead

    SELECT users.*,
           tweets.*
      FROM tutorial.tweets tweets
      LEFT JOIN tutorial.twitter_users users
        ON users.user_id = tweets.tweeted_by_id

You can see exactly which rows are not matched because all of the values from the `tweets` table are null for those rows.

Whenever you perform a Left Join, your results will return all of the rows in the table that you specify in teh `FROM` clause, and only matching rows from the table in the `LEFT JOIN` clause.

###RIGHT JOIN
Right Joins do the opposite &mdash; They return all rows from the table in the `RIGHT JOIN` clause and only matching rows from the table in the `FROM` clause.

![Right Joins}(http://www.w3schools.com/sql/img_rightjoin.gif)

`RIGHT JOIN` is rarely used because you can achieve the results of a Right Join by simply switching the two joined table names in a Left Join. For example, this query produces the same results as the previous query:

    SELECT users.*,
           tweets.*
      FROM tutorial.twitter_users users
     RIGHT JOIN tutorial.tweets tweets
        ON users.user_id = tweets.tweeted_by_id

The convention of always using `LEFT JOIN` probably exists to make queries easier to read/audit, but beyond that there isn't necessarily a strong reason to avoid using Right Join.

Also, `LEFT JOIN` and `RIGHT JOIN` can be written as `LEFT OUTER JOIN` and `RIGHT OUTER JOIN`, respectively.

###Filtering in the ON clause
Let's go back to the `LEFT JOIN` example:

    SELECT users.*,
           tweets.*
      FROM tutorial.tweets tweets
      LEFT JOIN tutorial.twitter_users users
        ON users.user_id = tweets.tweeted_by_id

Normally, filtering is processed in the `WHERE` clause once the two tables have already been joined. It's possible, though that you might want to filter one or both of the tables before joining them. For example, you only want to create matches between the tables under certain circumstances. Compare the following query to the previous one and you will see that some rows were filtered pre-join and not matched to the `tutorial.tweets` table.

    SELECT users.*,
           tweets.*
      FROM tutorial.tweets tweets
      LEFT JOIN tutorial.twitter_users users
        ON users.user_id = tweets.tweeted_by_id
       AND 

   What's happening above is that the conditional statement `AND...` is evaluated

###Inequality joins


http://en.wikipedia.org/wiki/Join_(SQL)

http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins

<!--filtering using JOIN vs. WHERE clause -->

Check out the next lesson: [Full Join and UNION](/intermediate/full-join-union.html)