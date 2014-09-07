---
layout: sqlschool-lesson
category: "the-basics"
title:  "Basic Concepts"
date:   2014-01-01 00:00:58
seo-title: "Introduction"
description: "Ready to answer questions with data? This SQL tutorial is for you."
---

###What is SQL and why should I care?
SQL (Structured Query Language) is a programming language designed for managing data in a relational database. It's been around since the 1970s and is the most common method of accessing data in databases today. SQL has a variety of functions that allows its users to read, manipulate, and change data. Though SQL is commonly used by engineers in software development, it's also popular with data analysts for a few reasons:

 * It's semantically easy to understand and learn.
 * Because it can be used to access large amounts of data directly where it's stored, analysts don't have to copy data into other applications.
 * Compared to spreadsheet tools, analysis done in SQL is easy audit and replicate. For analysts, this means no more looking for the [cell with the typo in the formula](http://www.washingtonpost.com/blogs/wonkblog/wp/2013/04/16/is-the-best-evidence-for-austerity-based-on-an-excel-spreadsheet-error/).

SQL is great for performing the types of aggregations that you might normally do in an Excel pivot table&mdash;sums, counts, minimums and maximums, etc.&mdash;but over much larger datasets and on multiple tables at the same time.

###How do I pronounce SQL?

[We have no idea.](http://patorjk.com/blog/2012/01/26/pronouncing-sql-s-q-l-or-sequel/)

###What's a database?
From [Wikipedia](http://en.wikipedia.org/wiki/Database): A database is an organized collection of data.

There are many ways to organize a database and many different types of databases designed for different purposes. Mode's structure is fairly simple:

<!-- diagram showing schema/table/row+column -->

If you've used Excel, you should already be familiar with tables &mdash; they're similar to spreadsheets. Tables have rows and columns just like Excel, but are a little more rigid. Database tables, for instance, are always organized by column, and each column must have a unique name. To get a sense of this organization, the image below shows a sample table containing data from the 2010 Academy Awards:

<img src="/images/the-basics/sample-table.png" alt="{{ page.seo-title }}" title="{{ page.seo-title }}">

Broadly, within databases, tables are organized in [schemas](http://en.wikipedia.org/wiki/Database_schema "Database Schemas"). At Mode, we organize tables around the users who upload them, so each person has his or her own schema. Schemas are defined by usernames, so if your username is databass3000, all of the tables you upload will be stored under the databass3000 schema. For example, if databass3000 uploads a table on fish food sales called `fish_food_sales`, that table would be referenced as `databass3000.fish_food_sales`. You'll notice that all of the tables used in this tutorial series are prefixed with "tutorial." That's because they were uploaded by an account with that username.

###You're on your way!
Now that you're familiar with the basics, it's time to dive in and learn some SQL.