---
layout: sqlschool-lesson
category: "intermediate"
title:  "Introduction to Joins"
date:   2014-02-01 00:00:55
seo-title: "Introduction to Joins"
description: "First of five lessons on SQL JOIN conditions using real-world examples. Free, interactive SQL tutorials to develop your data analysis skills."
---

###Intro to Joins - Relational Concepts
Up to this point, we've only been working with one table at a time. The real power of SQL, however, comes from working with data from multiple tables at once. If you remember from the [basic concepts tutorial](/the-basics/basic-concepts.html), the tables you've been working with up to this point are all part of the same schema in a relational database. The term "relational database" refers to the fact that the tables within it "relate" to one another&mdash;they contain common identifiers that allow information from multiple tables to be combined easily.

To understand what joins are and why they are helpful, let's think about Twitter.

Twitter has to store a lot of data. Twitter could (hypothetically, of course) store its data in one big table in which each row represents one tweet. There could be one column for the content of each tweet, one for the time of the tweet, one for the person who tweeted it, and so on. It turns out, though, that identifying the person who tweeted is a little tricky. There's a lot to a person's Twitter identity&mdash;a username, a bio, followers, followees, and more. Twitter could store all of that data in a table like this:

<a href="/images/intermediate/tweet-table.png" class="with-caption image-link" alt="{{ page.seo-title }}" title="A sample of how Twitter's user data might look">
  <img src="/images/intermediate/tweet-table.png" />  
</a>

Let's say, for the sake of argument, that Twitter did structure their data this way. Every time you tweet, Twitter creates a new row in its database, with information about you and the tweet. 

But this creates a problem. When you update your bio, Twitter would have to change that information for every one of your tweets in this table. If you've tweeted 5,000 times, that means 5,000 changes. If many people on Twitter are making lots of changes at once, that's a lot of computation to support. Instead, it's much easier for Twitter to store everyone's profile information in a separate table. That way, whenever someone updates their bio, Twitter would only have to change one row of data instead of thousands.

In an organization like this, Twitter now has two tables. The first table&mdash;the users table&mdash;contains profile information, and has one row per user. The second table&mdash;the tweets table&mdash;contains tweet information, including the username of the person who sent the tweet. By matching&mdash;or *joining*&mdash;that username in the tweets table to the username in the users table, Twitter can still connect profile information to every tweet.

<!-- image of sample tables, one for users, one for tweets -->