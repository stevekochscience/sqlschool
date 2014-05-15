---
layout: sqlschool-lesson
category: "solutions-to-common-problems"
title:  "Working with Timezones"
date:   2014-04-01 00:00:56
---

Depending on where you live timezones can really mess up your analysis. Let's start by figuring out what time zone your database thinks it's in by running this:

    SELECT NOW()

The above query will work on Mode's public data warehouse, but some SQL databases use the following syntax instead:

    SELECT GetDate()

It turns out that Mode's public database uses the [UTC timezone](http://www.timeanddate.com/time/aboututc.html), which is relatively similar to GMT.

This can create problems when you want to aggregate and display information. For example, if you want to show a graph of website traffic each hour and most of your viewers live in the United States, your results will probably mislead you into thinking that most of your traffic occurs very late at night. To make things accurate, you should convert your timestamps to an appropriate timezone.

Different types of databases handle this differently. Here's how Postgres (and Mode public) handle timezone changes:

    SELECT NOW() AT TIME ZONE 'PST8PDT' AS pacific_time,
           NOW() AT TIME ZONE 'UTC-8' AS hong_kong

You can look up the correct string to indicate your time zone using [this handy reference](http://www.cs.berkeley.edu/CT/ag4.0/appendid.htm). You'll notice that `pacific_time` uses `'PST8PDT'`, which is an absolute time zone, while `hong_kong` uses `'UTC-8'`, which is relative to the UTC time zone. You can specify any number of hours relative to the UTC time zone, but it is often better to use absolute references because of how different time zones handle daylight savings time.

###Data Stored Without Time Zones
Most SQL databases can also accept times without any timezone information attached at all. **In fact, if you upload data to Mode, it is stored without a timezone.** So if you want to translate the time, you need to input the time zone that it should be stored in and the desired output time zone.

Take this data on the Capital Bike Share program in Washington D.C. We know that it should be in Eastern Standard Time, though it's stored without a timezone in Mode. Let's walk through an example to convert the times to Pacific Standard Time. First take a look at the table:

    SELECT start_time,
           start_station,
           end_station,
           duration
      FROM tutorial.dc_bikeshare_q1_2012

Now specify the time zone it should be stored in, as well as the timezone for the output:

    SELECT start_time,
           start_time AT TIME ZONE 'EST5EDT' AS start_time_with_tz,
           start_time AT TIME ZONE 'EST5EDT' AT TIME ZONE 'PST8PDT' AS start_time_with_pst_tz,
           start_station,
           end_station,
           duration
      FROM tutorial.dc_bikeshare_q1_2012

In the above query, the 2nd column (`start_time_with_tz`) is telling the database to interpret the `start_time` column as being in Eastern Standard Time. You'll probably notice in the results, though, that it is *displayed* as being 5 hours ahead of the unaltered `start_time` column. This is because Mode's public data warehouse lives in UTC time &mdash; it is automatically showing you the time in its default time zone.

The third column above solves this problem. It includes the same syntax as the 2nd column to inform the database that the column should be interpreted as Eastern Standard Time, and then adds syntax to instruct it to be displayed in Pacific Standard Time. The result, as you can see, is that `start_time_with_pst_tz` is 3 hours earlier than the original column, `start_time`.

You can [read more about this here](http://www.postgresql.org/docs/9.1/static/functions-datetime.html#FUNCTIONS-DATETIME-ZONECONVERT).