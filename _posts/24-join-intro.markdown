#####Intro to joins - relational concepts
If you remember from the [basic concepts tutorial](LINK), the tables you've been working with up to this point are all part of the same schema in a relational database. The term "relational database" referrs to the fact that the tables within it "relate" to one another &mdash; they contain common identifiers that allow information from multiple tables to be combined easily.

As a practical example, let's examine how Twitter might use relational databases (*note: completely hypothetical*). One way to store Twitter's data would be to have one big table in which each row represents one tweet. The columns would express the content of each tweet, the time they were tweeted, and the people who tweeted them. It turns out, though, that identifying the people who tweeted is a little tricky. There's a lot to a person's twitter identity &mdash; a username, a bio, followers, followees, etc. Does all that info go in this table with every tweet? It *could*, but it probably shouldn't.

* image showing how this table might look

Let's say, for the sake of argument, that Twitter did structure their table such that every tweet you make has all of your information in the same row. When you update your bio, Twitter's system would have to change that information in every row of the tweets table. If you've tweeted 5,000 times, that means 5,000 changes. Now imagine everyone on Twitter making lots of changes at once &mdash; that's a lot of computation to support. It would be easier to just stick your profile information in a separate table. That way, Twitter's system would only have to change one row of data if you update your bio.

Here's what it looks like to have personal information and tweet data in separate tables. In order to keep track of who made tweets, a user_id is used.

* image showing mapping of tweets table to users table

The user_id is a *join key*, meaning that it exists in both tables to map them to one another.

[LINK TO NEXT SECTION](LINK)