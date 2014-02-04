---
layout: sqlschool-lesson
category: "intermediate"
title:  "Inner Joins"
date:   2014-02-01 00:00:55
---

###First, an Example
In the [last tutorial](/intermediate/join-intro.html), we suggested that Twitter separate data on tweets and users into two separate tables. Unfortunately, we can't use Twitter's data in any working examples (for that, we'll have to wait for the NSA's SQL School) but we can look at a similar problem.

In the previous [lesson on conditional logic](/intermediate/case.html), we worked with a table, `benn.college_football_players`, that contained data on college football players. This table included data on players, including the each player's weight and the school that they played for. It didn't include much information on the school, however, such as the conference the school is in&mdash;that information is in a separate table, `benn.college_football_teams`. 

Let's say we want to figure out which conference has the highest average weight. Given that information is in two separate tables, how do you do that? A join!

    SELECT teams.conference AS conference,
           AVG(players.weight) AS average_weight
      FROM benn.college_football_players players
      JOIN benn.college_football_teams teams
        ON teams.school_name = players.school_name
     GROUP BY teams.conference
     ORDER BY AVG(players.weight) DESC

There's a lot of new stuff happening here, so we'll go step-by-step.

###Aliases
When performing joins, it's easiest to give your table names aliases. `benn.college_football_players` is pretty long and annoying to type&mdash;`players` is much easier. You can give a table an alias by adding a space after the table name and typing the alias. As with column names, best practice here is to use underscores instead of spaces and all lowercase letters.

Once you've given a table an alias, you can refer to columns in that table in the `SELECT` clause using the alias name. For example, the first column selected in the above query is `teams.conference`. Because of the alias, this is equivalent to `benn.college_football_teams.conference`: we're selecting the `conference` column in the `college_football_teams` table in `benn`'s schema.

###JOIN and ON
After the `FROM` statement, we have two new statements: `JOIN`, which is followed by a table name, and `ON`, which is followed a couple column names separated by an equals sign.

Though the `ON` statement comes after `JOIN`, it's a bit easier to explain it first. `ON` indicates how the two tables (the one after the `FROM` and the one after the `JOIN`) relate to each other. In this case, the school a player went to is indicated by the `school_name` field in the `benn.college_football_players` table. This maps to the `school_name` field in the `benn.colllege_football_teams` table. This is written as a conditional statement:

    ON teams.school_name = players.school_name

In plain English, this means:

> Join all rows from the `players` table on to rows in the `teams` table for which the `school_name` field in the `players` table is equal to the `school_name` field in the `teams` table.

What does this actually do? Let's take a look at one row to see what happens. This is the row in the players table for Wake Forest wide receiver Michael Campanaro:

![WF player](/images/intermediate/player-join-example.png)

During the join, SQL looks up the `school_name`&mdash;in this case, "Wake Forest"&mdash;in the `school_name` field of the `teams` table. If there's a match, SQL takes all five columns from the `teams` table and joins them to ten columns of the `players` table. The new result is a fifteen row table, and the row with Michael Campanaro looks like this:

![After join](/images/intermediate/player-after-join-example.png)

*Two columns are cut off from the image, but you can see the full result [here](https://stealth.modeanalytics.com/tutorial/reports/12fac4065977/runs/51234e84bc0d)*

When you run a query with a join, SQL performs the same operation as it did above to every row of the table after the `FROM` statement. To see the full table returned by the join, try running this query:

    SELECT *
      FROM benn.college_football_players players
      JOIN benn.college_football_teams teams
        ON teams.school_name = players.school_name

Note that `SELECT *` returns all of the columns from both tables, not just from the table after `FROM`. If you want to only return columns from one table, you can write `SELECT players.*` to return all the columns from the players table.

Once you've generated this new table after the join, you can use the same aggregate functions from the [previous lesson](/intermediate/aggregation-functions.html). By running an `AVG` function on player weights, and grouping by the `conference` field from the teams table, we can figure out each conference's average weight.

###INNER JOIN

In the example above, all of the players in the `players` table match to one school in the `teams` table. But what if the data isn't so clean? What if there are multiple schools in the `teams` table with the same name? Or if a player goes to a school that isn't in the `teams` table? 

If there are multiple schools in the `teams` table with the same name, each one of those rows will get joined to matching rows in the `players` table. Returing to the previous example with Michael Campanaro, if there were three rows in the `teams` table where `school_name = "Wake Forest"`, the join query above would return three rows with Michael Campanaro. The first ten columns of these rows&mdash;the columns from the `players` table&mdash;would all be the same. The next five columns would match each of the three rows from the `teams` table.

While SQL always handles this first case the same, how it handles the second case depends on the type of join in the query. In addition to identifying the table you want to join onto the table in the `FROM` clause, the `JOIN` identifies the type of join you want to perform. 

The most common type of join&mdash;an *inner join*&mdash;can be written as either `JOIN benn.college_football_teams teams` or `INNER JOIN benn.college_football_teams teams`. Inner Joins eliminate rows from both tables that do not satisfy the join condition set forth in the `ON` statement. In mathematical terms, an Inner Join is the *intersection* of the two tables.

![inner join](http://www.w3schools.com/sql/img_innerjoin.gif)

Therefore, if a player goes to a school that isn't in the `teams` table, that player won't be included in the result from an Inner Join. Similarly, if there are schools in the `teams` table that don't match to any schools in the `players` table, those rows won't be inlcuded in the results either.

If you want to keep the rows that aren't matched, you'll can use Outer Joins, which are covered in [next lesson](/intermediate/outer-joins.html).