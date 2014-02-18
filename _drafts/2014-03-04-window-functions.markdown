---
layout: sqlschool-lesson
category: "advanced"
title:  "Window Functions"
date:   2014-03-04 00:00:56
---

<!-- the dataset for this lesson...-->

###Intro to Window Functions
PostgreSQL's documentation does an excellent job of [introducing the concept of Window Functions](http://www.postgresql.org/docs/9.1/static/tutorial-window.html):

>A *window function* performs a calculation across a set of table rows that are somehow related to the current row. This is comparable to the type of calculation that can be done with an aggregate function. But unlike regular aggregate functions, use of a window function does not cause rows to become grouped into a single output row — the rows retain their separate identities. Behind the scenes, the window function is able to access more than just the current row of the query result.

The most practical example of this is a running total:

    SELECT *
      FROM 

You can see that the above query creates an aggregation (`running_total`) without using `GROUP BY`. Let's break down the syntax and see how it works.

###Basic Windowing Syntax
* OVER
* PARTITION BY
* ORDER BY

<div class="practice-prob">
  
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>

###Practical Uses for Windowing
<!-- more practical uses for windowing functions-->

Move on to the next lesson: [Making Queries Rub Faster](/advanced/faster-queries.html).