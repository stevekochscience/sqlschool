---
layout: sqlschool-lesson
category: "the-basics"
title:  "Logical Operators"
date:   2014-01-01 00:00:56
---

###Comparison Operators -- the weird ones
In the [previous tutorial](LINK), you played with some comparison opertors. There are a couple more that you're likely to find very useful. They're all special snowflakes, so we'll got through them individually:

###IN
You'll probably want to use `IN` pretty frequently &mdash; it allows you to specify a list of values that you'd like to include. For example, the following query will return results for which the age column is equal to one of the values in the list:

    SELECT *
      FROM tutorial.patient_list
     WHERE age IN (22, 37, 33, 54)

As in the previous tutorial, you can use non-numerical values, but they need to go inside single quotes. Regardless of the data type, the values in the list must be separated by commas. Here's another example:

    SELECT *
      FROM tutorial.patient_list
     WHERE physician_last_name IN ('Yamamoto', 'Honeydew', 'Chase')

###BETWEEN
This allows you to specify a range and select only rows within a certain range. It has to be paired with the `AND` operator, which you will learn about in just a few moments. Here's how `BETWEEN` looks:

    SELECT *
      FROM tutorial.patient_list
     WHERE age BETWEEN 30 AND 39

`BETWEEN` includes the values that you specify in the query, so the above query will return the exact same results as the following query:

    SELECT *
      FROM tutorial.patient_list
     WHERE age >= 30 AND columna <= 39

Some people prefer the latter example because it more explicitly shows what the query is doing (t's easy to forget whether or not BETWEEN includes the range bounds).

###IS NULL
Some tables contain null values &mdash; cells with no data in them at all. This can be confusing for heavy Excel users, because the difference between a cell having no data and a cell containing a space isn't meaningful in Excel. In SQL, the implications can be pretty serious. This is covered in greater detail in the [intermediate tutorial](LINK), but for now, here's what you need to know:

You can select rows that contain no data in a given column by using the `IS NULL` operator:

    SELECT *
      FROM tutorial.patient_list
     WHERE weight IS NULL

`WHERE weight = NULL` will **not** work &mdash; you can't perform arithmetic on null values.

###LIKE
`LIKE` allows you to match on similar values rather than exact ones. Using wildcard characters, you can define what must be exactly matched and what can be different. In this example, the results will include rows for which columnb starts with "San" and is followed by any number and selection of characters:

    SELECT *
      FROM tutorial.patient_list
     WHERE physician_last_name LIKE 'Sm%'

The `%` used above represents any character or set of characters. In this case, `%` is referred to as a "wildcard." In the SQL that Mode uses, `LIKE` is case-sensitive, meaning that the above query will only capture matches that start with a capital "S" and lower-case "m." To match in a way that is not case-sensitive, you can use the `ILIKE` command:

    SELECT *
      FROM tutorial.patient_list
     WHERE physician_last_name ILIKE 'sm%'

A few other wildcards allow you some flexibility in using the `LIKE` operator:

* SQL wildcards: http://www.w3schools.com/sql/sql_wildcards.asp

* Examples using some other wildcards

###Logical Operators
You'll likely want to filter using several conditions &mdash; possibly more often than you filter by only one condition. Logical operators allow you to use multiple comparison operators in one query.

###AND
`AND` will let you select only rows that satisfy two conditions. The following query will return only rows for which columna equals 5 and columnb is null:

    SELECT *
      FROM mode_sample_table_3
     WHERE columna = 5 AND columnb IS NULL

You can use `AND` with any comparison operator, and as many times as you want. If you run this query, you'll notice that all of the requirements are satisfied.

    SELECT *
      FROM mode_sample_table_3
     WHERE columna != 5
       AND columnb LIKE 'San%'
       AND columna >= 3

You can see that this example is spaced out onto multiple lines &mdash; a good way to make long `WHERE` clauses more readable.

###OR
If you want to select rows that satisfy either of two conditions, you can use `OR`. It works the same way as `AND`. Try running this:

    SELECT *
      FROM mode_sample_table_3
     WHERE columna < 10 OR columna > 20

You can combine `AND` with `OR` using parenthesis. The following query will return rows that satisfy **both** of the following conditions.

columna > 10
columnb = 'value1' OR columnb = 'value2 &mdash; because this line is in parentheses, you can think of it as its own condition.

    SELECT *
      FROM mode_sample_table_3
     WHERE columna > 10
       AND (columnb = 'value1' OR columnb = 'value2')
   
###NOT
You can add `NOT` before any conditional statement if you'd like to select the rows for which that statement is false. It works like this:

    SELECT *
      FROM mode_sample_table_3
     WHERE NOT columna < 10

Using `NOT` with `<` and `>` usually doesn't make sense because you can simply use the opposite comparative operator instead. `NOT` is more commonly used with `LIKE`

    SELECT *
      FROM mode_sample_table_3
     WHERE columnb NOT LIKE 'San%'

`NOT` is mostly commonly used to identify non-null rows. Here's how that looks:

    SELECT *
      FROM mode_sample_table_3
     WHERE columnb IS NOT NULL

* conclusion

Move on to the next segment: [ORDER BY](/the-basics/order-by.html).