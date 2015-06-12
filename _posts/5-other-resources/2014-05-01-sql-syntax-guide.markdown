---
layout: sqlschool-lesson
category: "other-resources"
title:  "SQL Syntax Guide"
date:   2014-05-01 00:00:59
seo-title: "SQL Syntax"
---

###Mode public SQL reference
Mode's public service runs on Postgres.

For security reasons, Mode's public service is read-only, so you can't use `CREATE TABLE`, `ALTER TABLE`, `DELETE`, or any other commands that might alter the underlying database. This narrows down your options to `SELECT` statements and a few others like `EXPLAIN`. This means that large parts of Amazon's documentation are irrelevant.

You can find the complete PostgreSQL syntax guide [here](http://www.postgresql.org/docs/9.3/static/sql.html).

If you are very new to SQL, you should check out [the beginner tutorial](/the-basics/basic-concepts.html "Learning SQL").

###Connected database SQL reference
You can [connect Mode to many types of databases](http://help.modeanalytics.com/connecting-to-databases/ "Connecting to a Database"), all of which have slightly different syntax rules. When you run a query on Mode, it gets executed on the connected database, not Mode's Postgres database, so you should determine what type of database you're querying and find the appropriate documentation.

To make that a bit easier, we've collected documentation for some popular database types. *Note: database connections are also read-only, so many of the commands listed in these reference guides will not work:*

* [MySQL](http://dev.mysql.com/doc/ "MySQL")
* [PostgreSQL](http://www.postgresql.org/docs/manuals/ "PostgreSQL")
* [Redshift](http://docs.aws.amazon.com/redshift/latest/dg/cm_chap_SQLCommandRef.html "Redshift")
* [Oracle](http://docs.oracle.com/cd/E11882_01/server.112/e41084/toc.htm "Oracle")
* [MS SQl Server (T-SQL)](http://technet.microsoft.com/en-us/library/ms189826.aspx)
* [HIVE](https://cwiki.apache.org/confluence/display/Hive/LanguageManual "HIVE")
* [Impala](http://www.cloudera.com/content/cloudera-content/cloudera-docs/Impala/latest/Installing-and-Using-Impala/ciiu_langref.html "Impala")
* [TeraData](http://www.info.teradata.com/templates/eSrchResults.cfm?prodline=&txtpid=&txtrelno=&txtttlkywrd=SQLREF,TNTSEQ&rdsort=Title&srtord=Asc&nm=SQL+Reference)
* [Vertica](https://my.vertica.com/docs/7.0.x/PDF/HP_Vertica_7.0.x_SQL_Reference_Manual.pdf)