# Intermediate SQL

These materials are meant to support classes that teach SQL topics beyond `select * from ____ where ____`. The format was originally meant for three full-day workshops, but is appropriate to be split into many smaller classes as well.

SQL is an enormous topic, and many subtopics could be considered intermediate or advanced. The topics chosen for this repository fit three main themes:

* Understanding the tradeoffs and decisions that database administrators have to make at a big picture level.
    * Workload type (write heavy? read heavy?)
    * Security
    * Infrastructure/architecture
    * Resource contention
    * Schema design
* Understanding the performance of SQL queries
    * The query planner
    * Indexing
    * Types of joins (e.g. sort-merge vs hash vs index)
* Making more complex queries
    * Aggregates and window functions
    * Regular expressions (regex)
    * Set operations
    * Common table expressions

## Setup & Installation

* Install the latest version of Postgres from their website. This material ought to work for older versions, but was only tested on Postgres 13.
    * [https://www.postgresql.org/download/](https://www.postgresql.org/download/)

* Optional, but suggested: Install a query tool that is compatible with Postgres.
    * [PGAdmin](https://www.pgadmin.org/download/) is the de facto Postgres query tool.
    * [DBeaver](https://dbeaver.io/) is a query tool that supports many DBMSs (database mangement systems).
    * Some IDE's such as [Visual Studio Code](https://code.visualstudio.com/) have plugins to support querying databases such as [this one](https://marketplace.visualstudio.com/items?itemName=ckolkman.vscode-postgres)
    * There are many more options, but they all provide similar functionality. If you have a query tool you like, use it. If you aren't sure what to do, just use PGAdmin for now.

## Advice For Instructors

* The suggested order of the material is clearly denoted in the filenames.
* The general flow is for these materials is:
    * Demonstration 
    * Exercise
    * Review the solution
    * Repeat
* Many of the exercises assume work in small groups, particularly in the first module where the exercises are discussion oriented.

## Advice For Self Study Students

* The demonstration sections are written with knowledgeable instructors in mind, and might not contain all the information you'd need to complete an exercise.
    * Consider doing additional research, there are many SQL tutorials out there!
    * Get help from a friend, social network, or elsewhere (Teb's Lab contract information is on our website...).
    * Try to look at the solutions only as a last resort.

## License

All of the material in this repository is dedicated to the public domain. See the `LICENSE` file for more details.

## Support Free Curricula and Teb's Lab

These materials were created by [Teb's Lab](https://tebs-lab.com). We maintain several open source curriculum repositories, all with public domain dedications. Browse our repos on [Github](https://github.com/Tebs-Lab/), or [our website](https://www.tebs-lab.com/education).

You can support the creation of more free, open source, public domain educational materials by sharing them with others, [subscribing to our newsletter](http://eepurl.com/dum8IP), becoming a subscriber on [Patreon](https://www.patreon.com/tebsLab), or [arranging a training from Teb's Lab](https://www.tebs-lab.com/contracting).