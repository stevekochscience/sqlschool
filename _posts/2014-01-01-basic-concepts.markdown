---
layout: sqlschool-lesson
category: "the-basics"
title:  "Basic Concepts"
date:   2014-01-01 00:00:59
---

###Who is this tutorial for?
This tutorial is designed for data analysis, and ignores all functions that might alter the data permanently. If you're a software developer looking for a more complete SQL tutorial, this might be a good place to start, but certainly won't include everything you'll need to know. In this beginner tutorial, we'll assume that you have used Excel a little bit, but have no SQL experience. If you're already somewhat familiar with SQL, check out the [table of contents](/) and start in the place that's right for you.

###What is SQL and why should I care?
SQL (Structured Query Language) is a programming language designed for managing data in a relational database. It's been around since the 1970s and is the most common method of accessing data in databases today. SQL has a variety of functions that allows its users to read, manipulate, and change data. SQL is popular for data analysis for a few reasons:

* It is semantically easy to understand and learn
* It can be used to access large amounts of data directly where it is stored very easily
* It is easy to audit and repeat analysis (relative to Excel & similar tools)

SQL is great for performing the types of aggregations that you might normally do in an Excel pivot table &mdash; SUM, COUNT, MIN, MAX, etc. &mdash; but over much larger datasets and on multiple tables at the same time.

###Wait, so what's a database?
From [Wikipedia](http://en.wikipedia.org/wiki/Database): A database is an organized collection of data. There are many ways to organize a database and many different types of databases designed for different purposes. Mode's structure is fairly simple:

* diagram showing schema/table/row+column

If you've used Excel, you should already be familiar with tables &mdash; you can think of them as spreadsheets. Tables have rows and columns just like Excel, but are a little more rigid. For example, each column must have a name, where names are not required in Excel. Because of this ridigity, tables tend to look really similar at a glance, even if they contain totally unrelated data.

[Schemas](http://en.wikipedia.org/wiki/Database_schema "Database Schemas") are probably new for you. Broadly, the word "schema" refers to the way tables are organized in a database. At Mode, we decided to organize tables around the users who upload them, so each person has his or her own schema. Schemas are defined by usernames, so if your username is datadude55, all of the tables you upload will be named something like *datadude55.tablename*. You'll notice that all of the tables used in this tutorial series are prefixed with "tutorial." That's because they were uploaded by an account with that username.

###You're on your way!
Now that you're familiar with the basics, it's time to dive in and learn some SQL.

Move on to the next segment: [SELECT and FROM](/the-basics/select-from.html).
