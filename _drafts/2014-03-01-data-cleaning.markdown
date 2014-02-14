---
layout: sqlschool-lesson
category: "advanced"
title:  "Cleaning Messy Data"
date:   2014-03-01 00:00:58
---
###Cleaning Messy Data
* data cleaning functions

-string functions
-math functions



http://www.firstsql.com/tutor3.htm#exp

###Concatenation
In addition to using `CAST` and its equivalents to reformat problematic columns, you can use it to ensure correct formatting in a column that you create.

* COALESCE
* CONCAT ||
* SUBSTRING/ STRPOS & POSITION
* TRIM/LEFT/RIGHT/
* Changing string case with UPPER and LOWER (p109)

###Cleaning Dates:
-data type formatting functions
  -converting shitty dates (especially from Excel) into good ones
  
  extracting parts of a date from a date field
    EXTRACT(DAY FROM fieldname)
    
    
DATE_TRUNC('day', fieldname)

options: day, decade, dow (day of week 0-6), doy (day of year 1-365/366), 
hour, minute, month, quarter, second, week, year

Timezone stuff
AT TIME ZONE
CURRENT & LOCAL STUFF (most of the way down this page: http://www.postgresql.org/docs/8.1/static/functions-datetime.html)