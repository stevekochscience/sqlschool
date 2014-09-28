---
layout: sqlschool-lesson
category: "common-problems"
title:  "Pivoting From Columns to Rows"
date:   2014-04-01 00:00:57
seo-title: "Pivot Columns to Rows"
description: "Advanced SQL lesson: learn to pivot columns to rows. Free, interactive SQL tutorials with real-world examples to develop your data analysis skills."
---

A lot of data you'll find out there on the internet is formatted for consumption, not analysis. Take, for example, [this table showing the number of earthquakes worldwide from 2000-2012](http://earthquake.usgs.gov/earthquakes/eqarchives/year/eqstats.php):

<a href="/images/common-problems/earthquake-table.png" class="with-caption image-link" alt="{{ page.seo-title }}" title="Data that looks good in a presentation isn't always easy to work with">
  <img src="/images/common-problems/earthquake-table.png" />  
</a>

<!-- another example http://www.imf.org/external/pubs/ft/weo/2014/01/weodata/weorept.aspx?pr.x=57&pr.y=10&sy=2004&ey=2019&scsm=1&ssd=1&sort=country&ds=.&br=1&c=122%2C136%2C124%2C941%2C423%2C137%2C939%2C181%2C172%2C138%2C132%2C182%2C134%2C936%2C174%2C961%2C178%2C184&s=NGDP_R&grp=0&a=-->

In this format it's challenging to answer questions like "what's the average magnitude of an earthquake?" It would be much easier if the data were displayed in 3 columns: "magnitude", "year", and "number of earthquakes." Here's how to transfor the data into that form:

First, check out this data in Mode:

    SELECT *
      FROM tutorial.worldwide_earthquakes

*Note: column names begin with 'year_' because Mode requires column names to begin with letters.*

The first thing to do here is to create a table that lists all of the columns from the original table as rows in a new table. Unless you have a ton of columns to transform, the easiest way is often just to list them out in a subquery:

    SELECT year
      FROM (VALUES (2000),(2001),(2002),(2003),(2004),(2005),(2006),
                   (2007),(2008),(2009),(2010),(2011),(2012)) v(year)

Once you've got this, you can cross join it with the `worldwide_earthquakes` table to create an expanded view:

    SELECT years.*,
           earthquakes.*
      FROM tutorial.worldwide_earthquakes earthquakes
     CROSS JOIN (
           SELECT year
             FROM (VALUES (2000),(2001),(2002),(2003),(2004),(2005),(2006),
                          (2007),(2008),(2009),(2010),(2011),(2012)) v(year)
           ) years

Notice that each row in the `worldwide_earthquakes` is replicated 13 times. The last thing to do is to fix this using a `CASE` statement that pulls data from the correct column in the `worldwide_earthquakes` table given the value in the `year` column:

    SELECT years.*,
           earthquakes.magnitude,
           CASE year
             WHEN 2000 THEN year_2000
             WHEN 2001 THEN year_2001
             WHEN 2002 THEN year_2002
             WHEN 2003 THEN year_2003
             WHEN 2004 THEN year_2004
             WHEN 2005 THEN year_2005
             WHEN 2006 THEN year_2006
             WHEN 2007 THEN year_2007
             WHEN 2008 THEN year_2008
             WHEN 2009 THEN year_2009
             WHEN 2010 THEN year_2010
             WHEN 2011 THEN year_2011
             WHEN 2012 THEN year_2012
             ELSE NULL END
             AS number_of_earthquakes
      FROM tutorial.worldwide_earthquakes earthquakes
     CROSS JOIN (
           SELECT year
             FROM (VALUES (2000),(2001),(2002),(2003),(2004),(2005),(2006),
                          (2007),(2008),(2009),(2010),(2011),(2012)) v(year)
           ) years

[View the final product in Mode](https://modeanalytics.com/tutorial/reports/841a4e0ba1c7)

