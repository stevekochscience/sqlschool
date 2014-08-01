---
layout: sqlschool-lesson
category: "SQL QUIZ"
title:  "THE SQL QUIZ"
date:   2014-04-01 00:00:57
---

###Fun SQL quiz time

1. Here's one: I was recently asked to create a query that "generates the distribution for some quantity". Using the college football players database for example, write a query that shows how many schools have one player in the database, how many schools have two players in the database, three, four, etc. In other words, create a data needed for a histogram of the number of players on a team.


2. SELECT CASE WHEN year = 'FR' THEN 'FR' 
WHEN year = 'SO' THEN 'SO' 
WHEN year = 'JR' THEN 'JR' 
WHEN year = 'SR' THEN 'SR' 
ELSE 'No Year Data' END AS year_group, 
COUNT(1) AS count 
FROM benn.college_football_players 
GROUP BY 1