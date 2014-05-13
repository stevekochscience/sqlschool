---
layout: sqlschool-lesson
category: "solutions-to-common-problems"
title:  "Pivoting From Rows to Columns"
date:   2014-04-01 00:00:58
---

This lesson will teach you how to take data that is formatted for analysis and pivot it for presentation or charting. We'll take a dataset that looks like this:

![The Raw Data](/images/common-problems/pivot-step-one.png)

And make it look like this:

![The Pivoted Table](/images/common-problems/finished-pivot.png)

For this example, we'll use the same dataset of College Football players used in the [CASE lesson](/intermediate/case,html). You can view the data directly [here](https://modeanalytics.com/benn/tables/college_football_players).

Let's start by aggregating the data to show the number of players of each year in each conference, similar to the first example in the [inner join lesson](/intermediate/inner-joins.html):

    SELECT teams.conference AS conference,
           players.year,
           COUNT(1) AS players
      FROM benn.college_football_players players
      JOIN benn.college_football_teams teams
        ON teams.school_name = players.school_name
     GROUP BY 1,2
     ORDER BY 1,2

[View this in Mode](https://modeanalytics.com/tutorial/reports/18b97843ccda)

In order to transform the data, we'll need to put the above query into a subquery. It can be helpful to create the subquery and select all columns from it before starting to make transformations. Re-running the query at incremental steps like this makes it easier to debug if your query doesn't run. Note that you can eliminate the `ORDER BY` clause from the subquery since we'll reorder the results in the outer query.

    SELECT *
      FROM (
            SELECT teams.conference AS conference,
                   players.year,
                   COUNT(1) AS players
              FROM benn.college_football_players players
              JOIN benn.college_football_teams teams
                ON teams.school_name = players.school_name
             GROUP BY 1,2
           ) sub

Assuming that works as planned (results should look exactly the same as the first query), it's time to break the results out into different columns for various years. Each item in the `SELECT` statement creates a column, so you'll have to create a separate column for each year:

    SELECT conference,
           SUM(CASE WHEN year = 'FR' THEN players ELSE NULL END) AS fr,
           SUM(CASE WHEN year = 'SO' THEN players ELSE NULL END) AS so,
           SUM(CASE WHEN year = 'JR' THEN players ELSE NULL END) AS jr,
           SUM(CASE WHEN year = 'SR' THEN players ELSE NULL END) AS sr
      FROM (
            SELECT teams.conference AS conference,
                   players.year,
                   COUNT(1) AS players
              FROM benn.college_football_players players
              JOIN benn.college_football_teams teams
                ON teams.school_name = players.school_name
             GROUP BY 1,2
           ) sub
     GROUP BY 1
     ORDER BY 1

Technically, you've now accomplished the goal of this tutorial. But this could still be made a little better. You'll notice that the above query produces a list that is ordered alphabetically by Conference. It might make more sense to add a "total players" column and order by that (largest to smallest):

    SELECT conference,
           SUM(players) AS total_players,
           SUM(CASE WHEN year = 'FR' THEN players ELSE NULL END) AS fr,
           SUM(CASE WHEN year = 'SO' THEN players ELSE NULL END) AS so,
           SUM(CASE WHEN year = 'JR' THEN players ELSE NULL END) AS jr,
           SUM(CASE WHEN year = 'SR' THEN players ELSE NULL END) AS sr,
      FROM (
            SELECT teams.conference AS conference,
                   players.year,
                   COUNT(1) AS players
              FROM benn.college_football_players players
              JOIN benn.college_football_teams teams
                ON teams.school_name = players.school_name
             GROUP BY 1,2
           ) sub
     GROUP BY 1
     ORDER BY 2 DESC

And you're done! [View this in Mode](https://modeanalytics.com/tutorial/reports/47f2a54fb64a)

<!-- ####Making Charts -->
