* note: this one feels like it could come earlier

#####DISTINCT
You'll occasionally want to look at only the unique values in a particular column. You can do this using `SELECT DISTINCT` syntax:

    SELECT DISTINCT columna
      FROM mode_sample_table_6

If you include 2 (or more) columns in a `SELECT DISTINCT` clause, your results will contain all of the unique pairs of those two columns:

    SELECT DISTINCT columna, columnb
      FROM mode_sample_table_6

*Note: You only need to include `DISTINCT` once in your `SELECT` clause &mdash; you do not need to add it for each column name.*

#####Using  DISTINCT in aggregations
You can use `DISTINCT` when performing an aggregation. You will probably use it most commonly with the `COUNT` function:

* maybe include a practical example here

    SELECT COUNT(DISTINCT columna) AS distinct_count_a
      FROM mode_sample_table_6

You'll notice that `DISTINCT` goes inside the aggregation function rather than at the beginning of the `SELECT` clause.

Of course, you can `SUM` or `AVG` the distinct values in a column, but there are fewer practical applications for them. For `MAX` and `MIN`, you probably shouldn't ever use `DISTINCT` because the results will be the same as without `DISTINCT`, and the `DISTINCT` function will make your query substantially slower to return results.

#####DISTINCT Performances
It is worth noting that using `DISTINCT` can slow your queries down quite a bit. We'll cover this in greater depth in the [advanced tutorial](LINK TO PERFORMANCE)

[LINK TO NEXT TUTORIAL](LINK TO NEXT)