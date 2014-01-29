#####The WHERE Clause
So you know how to view some data using `SELECT` and `FROM`. The next step is filtering the data using the `WHERE` clause. Here's what it looks like.

    SELECT *
      FROM tutorial.flight_revenue
     WHERE destination = 'SFO'

*Note: the clauses always need to be in this order*

The `WHERE` clause works in a plain-English way: the above query does the same thing as `SELECT * FROM mode.sample_table_2`, except that the results will only include rows where the **fieldname** column contains the value **'something'**. Try running it both ways and note the differences.

**This part is really important: SQL deals with filtering an entire row at a time. In Excel, you can sort or filter one column at a time. In SQL, if you filter something in one column, entire rows will be omitted.** The idea is that each row is one *data point* or *observation*, and all the information contained in that row belongs together.

You can filter your results in a number of ways using comparison and logical operators.

#####Comparison Operators
The most basic way to filter data is using comparison operators. The easiest way to understand them is to start by looking at the list of basic numerical operators:

* =
* <> or !=
* >
* <
* >=
* <=

These make the most sense when applied to numerical columns. For example:

    SELECT destination, coach_rev
      FROM tutorial.flight_revenue
     WHERE coach_rev > 15000

Try running that query with each of the operators in place of `>`. Try some values other than 5 to get a sense of how this works. When you're ready, try to answer this question:

> Which ----variable----- are greater than 60 years old?

Don't worry &mdash; we'll get to some more practical examples shortly. [Click here for the answer](LINK).

#####Comparisons on non-numerical data
All of the above operators work on non-numerical data as well. `=` and `!=` make perfect sense &mdash; they allow you to select rows that match (or don't match) any value. For example:

    SELECT destination, coach_rev
      FROM tutorial.flight_revenue
     WHERE destination != 'SFO'

There are some important rules when using these operators, though. If you're using an operator with values that are non-numeric, you need to put the value in single quotes: `'value'`. *Note: SQL uses single quotes (as opposed to double quotes) in every instance except one, which we will get to in a later tutorial.*

You can use `>`, `<`, and the rest of the operators on non-numeric columns as well &mdash; they filter based on alphabetical order. Try running this a couple times with different operators:

    SELECT destination, coach_rev
      FROM tutorial.flight_revenue
     WHERE destination > 'n'

The way SQL treats alphabetical ordering is a little bit tricky. You may have noticed in the above query that selecting `destination > 'n'` will yield only rows in which fieldname starts with "n" or later laters in the alphabet. "Wait a minute," you might say. "If I wanted to include value that start with n, I would have used `fieldname >= 'n'`." SQL considers "na" to be greater than "n" because it has an extra letter. It's worth noting that most dictionaries would list "na" after "n" as well.

#####Arithmetic
* +, -, etc.

#####Practice time
If you'd like a little practice, try these questions:

* QUESTION
* QUESTION

If you're ready to mode on [Click here to move on to the next lesson: Logical Operators](LINK)
