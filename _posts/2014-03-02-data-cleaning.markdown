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

<div id="string-cleaning"></div>
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

<div id="trim"></div>
When using functions within other functions, it's important to remember that the innermost functions will be evaluated first, followed by the functions that encapsulate them.

####TRIM
The `TRIM` function is used to remove characters from the beginning and end of a string. Here's an example:

    SELECT location,
           TRIM(both '()' FROM location)
      FROM tutorial.sf_crime_incidents_2014_01

<div id="strpos"></div>
The `TRIM` function takes 3 arguments. First, you have to specify whether you want to remove characters from the beginning ('leading'), the end ('trailing'), or both ('both', as used above). Next you must specify all characters to be trimmed. Any characters included in the single quotes will be removed from both the beginning and end of the string. Finally, you must specify the text you want to trim using `FROM`.

####POSITION and STRPOS
`POSITION` allows you to specify a substring, then returns a numerical value equal to the character number (counting from left) where that substring first appears in the target string. For example, the following query will return the position of the character 'a' where it first appears in the `descript` field:

    SELECT incidnt_num,
           descript,
           POSITION('A' IN descript) AS a_position
      FROM tutorial.sf_crime_incidents_2014_01

You can also use the STRPOS function to achieve the same results &mdash; just replace "IN" with a comma and switch the order of the string and substring:

<div id="substr"></div>

    SELECT incidnt_num,
           descript,
           STRPOS(descript, 'A') AS a_position
      FROM tutorial.sf_crime_incidents_2014_01
  
####SUBSTR
`LEFT` and `RIGHT` both create substrings of a specified length, but they only do so starting from the sides of an existing string. If you want to start in the middle of a string, you can use `SUBSTR`. The syntax is `SUBSTR(*string*, *starting character position*, *# of characters*)`:

    SELECT incidnt_num,
           date,
           SUBSTR(date, 4, 2) AS day
      FROM tutorial.sf_crime_incidents_2014_01

<div id="concat"></div>
<div class="practice-prob">
  Write a query that separates the `location` field into separate fields for lattitude and longitude. You can compare your results against the actual `lat` and `lon` fields in the table.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/78d533ed005c" target="_blank">See the Answer &raquo;</a>
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
  <a href="https://modeanalytics.com/tutorial/reports/3b594976c097" target="_blank">See the Answer &raquo;</a>
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
  <a href="https://modeanalytics.com/tutorial/reports/1dc767c9846d" target="_blank">See the Answer &raquo;</a>
</div>

<div id="upper-lower"></div>
<div class="practice-prob">
  Write a query that creates a date column formatted YYYY-MM-DD.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/c0d258cf7b6a" target="_blank">See the Answer &raquo;</a>
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
  <a href="https://modeanalytics.com/tutorial/reports/c96ee5c6516d" target="_blank">See the Answer &raquo;</a>
</div>

There are a number of variations of these functions, as well as several other string functions not covered here. Different databases use subtle variations on these functions, so be sure to look up [the appropriate database's syntax](/other-resources/sql-syntax-guide.html) if you're connected to a private database. If you're using Mode's public service as in this tutorial, the [Postgres literature](http://www.postgresql.org/docs/9.1/static/functions-string.html) contains the related functions.

<!-- possibly split into 2 lessons right here -->

###Turning Strings into Dates
Dates are some of the most commonly screwed-up formats in SQL. This can be the result of a few things:

* The data was manipulated in Excel at some point, and the dates were changed to MM/DD/YYYY format or another format that is not compliant with SQL's strict standards.
* The data was manually entered by someone who use whatever formatting convention he/she was most familiar with.
* The date uses text (Jan, Feb, etc.) intsead of numbers to record months.

In order to take advantage of all of the great date functionality (`INTERVAL`, as well as some others you will learn in the next secion), you need to have your date field formatted appropriately. This often involves some text manipulation, followed by a `CAST`. Let's revisit the answer to one of the practice problems above:

    SELECT incidnt_num,
           date,
           (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) ||
            '-' || SUBSTR(date, 4, 2))::date AS cleaned_date
      FROM tutorial.sf_crime_incidents_2014_01

This example is a little different from the answer above in that we've wrapped the entire set of concatenated substrings in parentheses and cast the result in the `date` format. We could also case it as  `timestamp`, which includes additional precision (hours, minutes, seconds). In this case, we're not pulling the hours out of the original field, so we'll just stick to `date`.

<!-- example showing how to pull apart a shitty string with months spelled out into its parts-->

<div class="practice-prob">
  Write a query that creates an accurate timestamp using the <code>date</code> and <code>time</code> columns in <code>tutorial.sf_crime_incidents_2014_01</code>. Include a field that is exactly 1 week later as well.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/4c908f47868a" target="_blank">See the Answer &raquo;</a>
</div>

###Turning Dates into More Useful Dates
Once you've got a well-formatted date field, you can manipulate in all sorts of interesting ways. To make the lesson a little cleaner, we'll use a different version of the crime incidents dataset that already has a nicely-formatted date field:

    SELECT *
      FROM tutorial.sf_crime_incidents_cleandate

<div id="extract"></div>
You've learned how to construct a date field, but what if you want to deconstruct one? You can use `EXTRACT` to pull the pieces apart one-by-one:


    SELECT cleaned_date,
           EXTRACT('year'   FROM cleaned_date) AS year,
           EXTRACT('month'  FROM cleaned_date) AS month,
           EXTRACT('day'    FROM cleaned_date) AS day,
           EXTRACT('hour'   FROM cleaned_date) AS hour,
           EXTRACT('minute' FROM cleaned_date) AS minute,
           EXTRACT('second' FROM cleaned_date) AS second,
           EXTRACT('decade' FROM cleaned_date) AS decade,
           EXTRACT('dow'    FROM cleaned_date) AS day_of_week
      FROM tutorial.sf_crime_incidents_cleandate

You can also round dates to the nearest unit of measurement. This is particularly useful if you don't care about an individual date, but do care about the week (or month, or quarter) that it occurred in. The `DATE_TRUNC` function rounds a date to whatever precision you specify. The value displayed is the first value in that period. So when you `DATE_TRUNC` by year, any value in that year will be listed as January 1st of that year:

    SELECT cleaned_date,
           DATE_TRUNC('year'   , cleaned_date) AS year,
           DATE_TRUNC('month'  , cleaned_date) AS month,
           DATE_TRUNC('week'   , cleaned_date) AS week,
           DATE_TRUNC('day'    , cleaned_date) AS day,
           DATE_TRUNC('hour'   , cleaned_date) AS hour,
           DATE_TRUNC('minute' , cleaned_date) AS minute,
           DATE_TRUNC('second' , cleaned_date) AS second,
           DATE_TRUNC('decade' , cleaned_date) AS decade
      FROM tutorial.sf_crime_incidents_cleandate

<div class="practice-prob">
  Write a query that counts the number of incidents reported by week. Cast the week as a date to get rid of the hours/minutes/seconds.
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/0315a8aa1e4c" target="_blank">See the Answer &raquo;</a>
</div>

What if you want to include today's date or time? You can instruct your query to pull the local date and time at the time the query is run using any number of functions. Interestingly, you can run them without a `FROM` clause:

    SELECT CURRENT_DATE AS date,
           CURRENT_TIME AS time,
           CURRENT_TIMESTAMP AS timestamp,
           LOCALTIME AS localtime,
           LOCALTIMESTAMP AS localtimestamp,
           NOW() AS now

As you can see, the different options vary in precision. You might notice that these times probably aren't actually your local time. Mode's database is set to [Coordinated Universal Time](http://en.wikipedia.org/wiki/Coordinated_Universal_Time) (UTC), which is basically the same as GMT. If you run a current time function against a connected database, you might get a result in a different time zone.

You can make a time appear in a different time zone using `AT TIME ZONE`:

    SELECT CURRENT_TIME AS time,
           CURRENT_TIME AT TIME ZONE 'PST' AS time_pst

For a complete list of timezones, [look here](http://www.postgresql.org/docs/7.2/static/timezones.html). This functionality is pretty complex because timestamps can be stored with or without timezone metadata. For a better understanding of the exact syntax, we recommend checking out the [Postgres documentation](http://www.postgresql.org/docs/9.2/static/functions-datetime.html#FUNCTIONS-DATETIME-ZONECONVERT).

<div id="coalesce"></div>
<div class="practice-prob">
  Write a query that shows exactly how long ago each indicent was reported. Assume that the dataset is in Pacific Standard Time (UTC - 8).
</div>
<div class="practice-prob-answer">
  <a href="https://modeanalytics.com/tutorial/reports/ebc77b3a1dd7" target="_blank">See the Answer &raquo;</a>
</div>

###COALESCE
Occasionally, you will end up with a dataset that has some nulls that you'd prefer to contain actual values. This happens frequently in numerical data (displaying nulls as 0 is often preferable), and when performing outer joins that result in some unmatched rows. In cases like this, you can use `COALESCE` to replace the null values:

    SELECT incidnt_num,
           descript,
           COALESCE(descript, 'No Description')
      FROM tutorial.sf_crime_incidents_cleandate
     ORDER BY descript DESC

<!--
<div class="practice-prob">
  Something involving a join
</div>
<div class="practice-prob-answer">
  <a href="" target="_blank">See the Answer &raquo;</a>
</div>
-->
Move on to the next lesson: [Subqueries](/advanced/subqueries.html).