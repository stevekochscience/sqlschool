---
layout: sqlschool-lesson
category: "advanced"
title:  "Wrangling Messy Data"
date:   2014-03-01 00:00:58
---
This lesson features data on San Francisco Crime Incidents for the 3-month period beginning November 1, 2013 and ending January 31, 2014. It was collected from the [SF Data website](https://data.sfgov.org/Public-Safety/Map-Crime-Incidents-Previous-Three-Months/gxxq-x39z) on February 16, 2014. There is one row for each indicent reported. Some field definitions: `location` is the GPS location of the incident, listed in [decimal degrees](http://en.wikipedia.org/wiki/Decimal_degrees), lattitude first, longitude second. The two coordinates are also broken out into the `lat` and `lon` fields, respectively. 

Start by taking a look:

    SELECT *
      FROM tutorial.sf_crime_incidents_2014_01

###What does it mean to "wrangle" data?
From [Wikipedia](http://en.wikipedia.org/wiki/Data_wrangling):

>Data munging or data wrangling is loosely the process of manually converting or mapping data from one "raw" form into another format that allows for more convenient consumption of the data with the help of semi-automated tools.

In other words, data wrangling (or munging) is the process of programmatically transforming data into a format that makes it easier to work with. This might mean modifying all of the values in a given column in a certain way, or merging multiple columns together. The necessity for data wrangling is often a biproduct of poorly collected or presented data. Data that is entered manually by humans is typically frought with errors; data collected from websites is often optimized to be displayed on websites, not to be sorted and aggregated.

If you work with SQL regularly, you'll need to become really comfortable with these skills, as they are what will allow you to get to the fun stuff.

###Cleaning Strings
Most of the functions presented in this lesson are specific to certain data types. However, using a particular function will, in many cases, change the data to the appropriate type. `LEFT`, `RIGHT`, and `TRIM` are all used to select only certain elements of strings, but using them to select elements of a number or date will turn treat them as strings for the purpose of the function.

####LEFT, RIGHT, and LENGTH
Let's start with `LEFT`. You can use `LEFT` to pull a certain number of characters from the left side of a string and present them as a separate string. The syntax is `LEFT(*string*, *number of characters*)`.

As a practical example, we can see that the `date` field in this dataset begins with a 10-digit date, and include the timestamp to the right of it. The following query pulls out only the date:

    SELECT incidnt_num,
           date,
           LEFT(date, 10) AS cleaned_date
      FROM tutorial.sf_crime_incidents_2014_01

`RIGHT` does the same thing, but from the right side:

    SELECT incidnt_num,
           date,
           LEFT(date, 10) AS cleaned_date,
           RIGHT(date, 17) AS cleaned_time
      FROM tutorial.sf_crime_incidents_2014_01

`RIGHT` works well in this case because we know that the number of characters will be consistent across the entire `date` field. If it wasn't consistent, it's still possible to pull a string from the right side in a way that makes sense. The `LENGTH` function returns the length of a string. So `LENGTH(date)` will always return `28` in this dataset. Since we know that the first 10 characters will be the date, and they will be followed by a space (total 11 characters), we could represent the `RIGHT` function like this:

    SELECT incidnt_num,
           date,
           LEFT(date, 10) AS cleaned_date,
           RIGHT(date, LENGTH(date) - 11) AS cleaned_time
      FROM tutorial.sf_crime_incidents_2014_01

When using functions within other functions, it's important to remember that the innermost functions will be evaluated first, followed by the functions that encapsulate them.

####TRIM
The `TRIM` function is used to remove characters from the beginning and end of a string. Here's an example:

    SELECT location,
           TRIM(both '()' FROM location)
      FROM tutorial.sf_crime_incidents_2014_01

The `TRIM` function takes 3 arguments. First, you have to specify whether you want to remove characters from the beginning ('leading'), the end ('trailing'), or both ('both', as used above). Next you must specify all characters to be trimmed. Any characters included in the single quotes will be removed from both the beginning and end of the string. Finally, you must specify the text you want to trim using `FROM`.

####POSITION and STRPOS
`POSITION` allows you to specify a substring, then returns a numerical value equal to the character number (counting from left) where that substring first appears in the target string. For example, the following query will return the position of the character 'a' where it first appears in the `descript` field:

    SELECT incidnt_num,
           descript,
           POSITION('a' IN descript) AS a_position
      FROM tutorial.sf_crime_incidents_2014_01

You can also use the STRPOS function to achieve the same results &mdash; just replace "IN" with a comma and switch the order of the string and substring:

    SELECT incidnt_num,
           descript,
           STRPOS(descript, 'a') AS a_position
      FROM tutorial.sf_crime_incidents_2014_01
  
####SUBSTR
`LEFT` and `RIGHT` both create substrings of a specified length, but they only do so starting from the sides of an existing string. If you want to start in the middle of a string, you can use `SUBSTR`. The syntax is `SUBSTR(*string*, *starting character position*, *# of characters*)`:

    SELECT incidnt_num,
           date,
           SUBSTR(date, 4, 2) AS day
      FROM tutorial.sf_crime_incidents_2014_01

<div class="practice-prob">
  Write a query that separates the `location` field into separate fields for lattitude and longitude. You can compare your results against the actual `lat` and `lon` fields in the table.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/78d533ed005c" target="_blank">See the Answer &raquo;</a>
</div>

####Concatenation
You can combine strings from several columns together (and with hard-coded values) using `CONCAT`. Simply order the values you want to concatenate and separte them with commas. If you want to hard-code values, enclose them in single quotes. Here's an example:

    SELECT incidnt_num,
           day_of_week,
           LEFT(date, 10) AS cleaned_date,
           CONCAT(day_of_week, ', ', LEFT(date, 10)) AS day_and_date
      FROM tutorial.sf_crime_incidents_2014_01

<div class="practice-prob">
  Concate the <code>lat</code> and <code>lon</code> fields to form a field that is identical to the <code>location</code> field.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/3b594976c097" target="_blank">See the Answer &raquo;</a>
</div>

Alternatively, you can use two pipe characters (`||`) to perform the same concatenation:

    SELECT incidnt_num,
           day_of_week,
           LEFT(date, 10) AS cleaned_date,
           day_of_week || ', ' || LEFT(date, 10) AS day_and_date
      FROM tutorial.sf_crime_incidents_2014_01

<div class="practice-prob">
  Create the same concatenated <code>location</code> field, but using the <code>||</code> syntax instead of <code>CONCAT</code>.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/1dc767c9846d" target="_blank">See the Answer &raquo;</a>
</div>

<div class="practice-prob">
  Write a query that creates a date column formatted YYYY-MM-DD.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/c0d258cf7b6a" target="_blank">See the Answer &raquo;</a>
</div>

####Changing Case with UPPER and LOWER
Sometimes, you just don't want your data to look like it's screaming at you. You can use `LOWER` to force every character in a string to become lower-case. Similarly, you can use `UPPER` to make all the letters appear in upper-case:

    SELECT incidnt_num,
           address,
           UPPER(address) AS address_upper,
           LOWER(address) AS address_lower
      FROM tutorial.sf_crime_incidents_2014_01

<div class="practice-prob">
  Write a query that returns the `category` field, but with the first letter capitalized and the rest of the letters in lower-case.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/c96ee5c6516d" target="_blank">See the Answer &raquo;</a>
</div>

There are a number of variations of these functions, as well as several other string functions not covered here. Different databases use subtle variations on these functions, so be sure to look up [the appropriate database's syntax](LINK) if you're connected to a private database. If you're using Mode's public service as in this tutorial, the [Postgres literature](http://www.postgresql.org/docs/9.1/static/functions-string.html) contains the related functions.

<!-- possibly split into 2 lessons right here -->

<!--
###Turning Strings into Dates
* most commonly screwed up (blame Excel)
* SQL works well with dates, but need to be formatted properly (as in previous lesson)
* example showing how to pull apart a shitty string into its parts

* practice problem: create a date field formatted properly (cast it as timestamp). add another field that is 1 year ahead using INTERVAL to prove it's a real timestamp.

###Turning Dates into More Useful Dates
  extracting parts of a date from a date field
    EXTRACT(DAY FROM fieldname)
    
    
DATE_TRUNC('day', fieldname)

options: day, decade, dow (day of week 0-6), doy (day of year 1-365/366), 
hour, minute, month, quarter, second, week, year

Timezone stuff
AT TIME ZONE
CURRENT & LOCAL STUFF (most of the way down this page: http://www.postgresql.org/docs/8.1/static/functions-datetime.html)

###COALESCE
* blah...
-->

<!-- add nulls to dataset so that COALESCE does something

<div class="practice-prob">
  Write a query that returns the `category` field, but with the first letter capitalized and the rest of the letters in lower-case.
</div>
<div class="practice-prob-answer">
  <a href="https://stealth.modeanalytics.com/tutorial/reports/c96ee5c6516d" target="_blank">See the Answer &raquo;</a>
</div>
-->
Move on to the next lesson: [Subqueries](/advanced/subqueries.html).