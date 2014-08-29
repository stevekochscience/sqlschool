---
layout: sqlschool-lesson
category: "intermediate"
title:  "Inner Joins"
date:   2014-02-01 00:00:54
seo-title: "JOIN"
description: "Learn to use the SQL JOIN clause with real-world examples. Free, interactive SQL tutorials to develop your data analysis skills."
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
When performing joins, it's easiest to give your table names aliases. `benn.college_football_players` is pretty long and annoying to type&mdash;`players` is much easier. You can give a table an alias by adding a space after the table name and typing the intended name of the alias. As with column names, best practice here is to use all lowercase letters and underscores instead of spaces.

Once you've given a table an alias, you can refer to columns in that table in the `SELECT` clause using the alias name. For example, the first column selected in the above query is `teams.conference`. Because of the alias, this is equivalent to `benn.college_football_teams.conference`: we're selecting the `conference` column in the `college_football_teams` table in `benn`'s schema.

<div id="join"></div>
<div class="practice-prob">
  Write a query that selects the school name, player name, position, and weight for every player in Georgia, ordered by weight (heaviest to lightest). Be sure to make an alias for the table, and to reference all column names in relation to the alias.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/b4bc413f9399" target="_blank">See the Answer &raquo;</a>
</div>

###JOIN and ON
After the `FROM` statement, we have two new statements: `JOIN`, which is followed by a table name, and `ON`, which is followed by a couple column names separated by an equals sign.

Though the `ON` statement comes after `JOIN`, it's a bit easier to explain it first. `ON` indicates how the two tables (the one after the `FROM` and the one after the `JOIN`) relate to each other. You can see in the example above that both tables contain fields called `school_name`. Sometimes relational fields are slightly less obvious. For example, you might have a table called "schools" with a field called "id", which could be joined against "school_id" in any other table. These relationships are sometimes called "mappings." `teams.school_name` and `players.school_name`, the two columns that map to one another, are referred to as "foreign keys" or "join keys." Their mapping is written as a conditional statement:

    ON teams.school_name = players.school_name

In plain English, this means:

> Join all rows from the `players` table on to rows in the `teams` table for which the `school_name` field in the `players` table is equal to the `school_name` field in the `teams` table.

What does this actually do? Let's take a look at one row to see what happens. This is the row in the players table for Wake Forest wide receiver Michael Campanaro:

<!-- DEREK: I think this section could better demonstrate join keys.  If you had images of both indivitual section first, then an image of them joined together with the key column highlighted in a different color/organized so it is the actual bridge point between the two tables, it would be easier for a reader to understand.  Text would of course need to be adjusted to reflect this.  Also, these small images are really hard to read.  I know the formatting is nice this way, but this is a pain in the ass for me, so I can only imagine what older users would think. -->


![WF player](/images/intermediate/player-join-example.png)

During the join, SQL looks up the `school_name`&mdash;in this case, "Wake Forest"&mdash;in the `school_name` field of the `teams` table. If there's a match, SQL takes all five columns from the `teams` table and joins them to ten columns of the `players` table. The new result is a fifteen column table, and the row with Michael Campanaro looks like this:

![After join](/images/intermediate/player-after-join-example.png)

*Two columns are cut off from the image, but you can see the full result [here](https://modeanalytics.com/tutorial/reports/12fac4065977/runs/51234e84bc0d)*

When you run a query with a join, SQL performs the same operation as it did above to every row of the table after the `FROM` statement. To see the full table returned by the join, try running this query:

    SELECT *
      FROM benn.college_football_players players
      JOIN benn.college_football_teams teams
        ON teams.school_name = players.school_name

Note that `SELECT *` returns all of the columns from both tables, not just from the table after `FROM`. If you want to only return columns from one table, you can write `SELECT players.*` to return all the columns from the players table.

Once you've generated this new table after the join, you can use the same aggregate functions from the [previous lesson](/intermediate/aggregation-functions.html). By running an `AVG` function on player weights, and grouping by the `conference` field from the teams table, we can figure out each conference's average weight.

###INNER JOIN

In the example above, all of the players in the `players` table match to one school in the `teams` table. But what if the data isn't so clean? What if there are multiple schools in the `teams` table with the same name? Or if a player goes to a school that isn't in the `teams` table? 

If there are multiple schools in the `teams` table with the same name, each one of those rows will get joined to matching rows in the `players` table. Returing to the previous example with Michael Campanaro, if there were three rows in the `teams` table where `school_name = 'Wake Forest'`, the join query above would return three rows with Michael Campanaro.

It is often the case that one or both tables being joined contain rows that don't have matches in the other table. The way this is handles depends on the type of query you're writing. More specifically, it depends on whether the join is an Inner or Outer join.

We'll start with Inner joins, which can be written as either `JOIN benn.college_football_teams teams` or `INNER JOIN benn.college_football_teams teams`. Inner Joins eliminate rows from both tables that do not satisfy the join condition set forth in the `ON` statement. In mathematical terms, an Inner Join is the *intersection* of the two tables.

![inner join](http://www.w3schools.com/sql/img_innerjoin.gif)

Therefore, if a player goes to a school that isn't in the `teams` table, that player won't be included in the result from an Inner Join. Similarly, if there are schools in the `teams` table that don't match to any schools in the `players` table, those rows won't be inlcuded in the results either.

###Tricky Stuff
When you join two tables, it might be the case that both tables have columns with identical names. In the below example, both tables have columns called `school_name`:

    SELECT players.*,
           teams.*
      FROM benn.college_football_players players
      JOIN benn.college_football_teams teams
        ON teams.school_name = players.school_name

The results can only support one column with a given name &mdash; when you include 2 columns of the same name, the results will simply show the exact same result set for both columns **even if the two columns should contain different data**. You can avoid this by naming the columns individually. It happens that these two columns will actually contain the same data because they are used for the join key, but the following query technically allows these columns to be independent:

    SELECT players.school_name AS players_school_name,
           teams.school_name AS teams_school_name
      FROM benn.college_football_players players
      JOIN benn.college_football_teams teams
        ON teams.school_name = players.school_name

###Practice

<div class="practice-prob">
  Write a query that displays player names, school names and conferences for schools in the "FBS (Division I-A Teams)" division.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/16915859fe4c" target="_blank">See the Answer &raquo;</a>
</div>

<!--
<div class="practice-prob">
  Join practice 2
</div>
<div class="practice-prob-answer">
  <a href="http://" target="_blank">See the Answer &raquo;</a>
</div>
-->