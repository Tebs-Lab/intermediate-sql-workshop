# Exercise: Restore and Explore Adventure Works

In the data folder of this repo you'll find a file called `adventureworks_backup`. This is a postgres dump of a mock database for a fake company called Adventure Works. The Adventure Works database is [maintained by Microsoft](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms) as a tool for practicing SQL on a realistic dataset for folks who might not otherwise have access to it. 

We'll use the Adventure Works database throughout this workshop. Your first task is to restore and explore the database using what you already know about SQL.

## Restore From The Dump

You can restore a PostgreSQL database from a dump using PGAdmin or the command line, whichever you prefer. See the documentation for:

* [Using the CLI](https://www.postgresql.org/docs/9.4/backup-dump.html)
* [PGAdmin](https://www.pgadmin.org/docs/pgadmin4/development/restore_dialog.html)

## Explore the Data

Once you've successfully restored the database, start exploring a few of the tables. Try to get an idea for what kind of products Adventure Works sells, how many employees they have, how many sales they make in a year, or whatever piques your interest. 

The purpose of this exercise is to get you thinking about schema design, and warm up to using SQL — which might be something you do only occasionally. 

This diagram of the Adventure Works database may help you during this exploration.

![](https://i.stack.imgur.com/LMu4W.gif)

