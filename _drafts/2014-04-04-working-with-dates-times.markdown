---
layout: sqlschool-lesson
category: "solutions-to-common-problems"
title:  "Working with Time Series Data"
date:   2014-04-01 00:00:56
---

Dates and timestamps are partially covered in the [Advanced Tutorial](/advanced/data-types.html), but there's so much left over as to necessitate a separate lesson. Thanks [Robert Berry](https://twitter.com/no0p_/) for the [inspiration for this post](http://no0p.github.io/postgresql/2014/05/08/timeseries-tips-pg.html).

###What do you mean "Time Series Data?"
-what it looks like
-some examples of when it occurs
-some examples of what people do with it

###Cleaning up timestamps with DATE_TRUNC
DATE_TRUNC



INTERVAL

filling in empty date ranges

    SELECT day, 0 as blank_count
      FROM 
    generate_series('2014-01-01 00:00'::timestamptz, current_date::timestamptz, '1 day') 
      as day

Time series and window functions
LEAD and LAG

Time zones are complicated enough to merit their own lesson. You can learn more about working with time zones [here](/solutions-to-common-problems/working-with-timezones.html).


reference: http://no0p.github.io/postgresql/2014/05/08/timeseries-tips-pg.html