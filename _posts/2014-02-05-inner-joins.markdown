---
layout: sqlschool-lesson
category: "intermediate"
title:  "Inner Joins"
date:   2014-02-01 00:00:55
---

#####First, an example
The Twitter example in the [previous tutorial](LINK) presents a problem: If you've got tweets and people in separate tables, what happens when you want to count the number of times each user has tweeted? The answer is that you use a join:

    SELECT users.username,
           COUNT(tweets.tweet_text)
      FROM tutorial.tweets tweets
      JOIN tutorial.twitter_users users
        ON users.user_id = tweets.tweeted_by_id
     GROUP BY users.username

There's a lot of new stuff happening here, so we'll go step-by-step.

#####Aliases
When performing joins, it's easiest to give your table names aliases. `tutorial.twitter_users` is pretty long and annoying to type &mdash; `users` would probably be easier. You can give a table an alias by adding a space after the table name and typing the alias. As with column names, best practice here is to use underscores instead of spaces and all lowercase letters.

Once you've given a table an alias, you can refer to columns in that table in the `SELECT` clause using the alias name. For example, the first column selected in the above query is `users.username`. Because of the alias, this is equivalent to `tutorial.twitter_users.username`.

#####ON
The `ON` statement must come after `JOIN`, but it's easiest to explain it first. `ON` indicates how the two tables map to one another. In this case, each tweet has a `tweeted_by_id` indicating the `user_id` of the user who made that tweet. This maps to the `user_id` field in the `tutorial.tweets` table. This is written as a conditional statement:

    ON users.user_id = tweets.tweeted_by_id

In plain English, this means:

> Join all rows from the `tweets` table on to rows in the `users` table for which the `user_id` field in the `users` table is equal to the `tweeted_by_id` field in the `tweets` tabls.

The result is almost like looking at the two tables side by side. Run these two queries.

    SELECT * FROM tutorial.tweets

    SELECT * FROM tutorial.twitter_users

Then run this query to see what the joined data looks like. **be sure to turn the limit off for this one**:

    SELECT users.*,
           tweets.*
      FROM tutorial.tweets tweets
      JOIN tutorial.twitter_users users
        ON users.user_id = tweets.tweeted_by_id

What you probably didn't notice is that some rows were actually eliminated when this happened. The `tweets` table has 198 rows, and the `users` table has 50 rows. So why does this output show **X**?

#####INNER JOIN
The `JOIN` clause serves two purposes:

1. It identifies the table that you want to join onto the table in the `FROM` clause.
2. It identifies the type of join you want to perform.

The `JOIN` clause is actually a specific type of join &mdash; an inner join. In fact, the above line `JOIN tutorial.twitter_users users` could also be written as `INNER JOIN tutorial.twitter_users users`.

What makes Inner Joins different from Outer Joins (we'll get to those shortly), is that Inner Joins eliminate rows from both tables that do not satisfy the join condition set forth in the `ON` statement. In mathematical terms, an Inner Join is the *intersection* of the two tables.

* IMAGE: http://www.w3schools.com/sql/img_innerjoin.gif

You can see that some rows in the `tutorial.tweets` table have `tweeted_by_id`s above 50, while the `twitter_users` table only has `user_id`s up to 50. This explains the reduction in rows you saw from 198 to **X** &mdash; all the rows from `tutorial.tweets` for which `tweeted_by_id` is higher than 50 were not displayed in the query results.

If you want to keep the rows that aren't matched, you'll need to use an Outer Join.

[LINK TO OUTER JOIN LESSON](LINK)