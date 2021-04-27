# Data Consumers

In this section we're going to describe the consumers of data. Consumers can be people as well as other programs. The enumeration of data consumers can be deceptively complex, and might entail long and convoluted data pipelines.

## Who and What Consumes Your Data?

* **Ask the students to list the consumers of data at their company, specifically the people and software that need some kind of read access to the databases.**
    * Diagram these out, just like you did with the data sources.
    * DBAs, obviously.
    * Data analytics teams.
        * Potentially directly via terminal/PGAdmin/Toad/Etc.
        * Potentially mediated by 3rd party software like Tableau/Spotfire/PowerBI
            * Reminder that these apps probably query their OWN web servers which then query the DB. It's unlikely the apps are sending queries directly to the DB.
    * Web/Phone apps that need to display things to the customers.
        * Likely mediated by a web server.
    * 3rd parties potentially
        * suppliers who want to check on their relationship with you.
        * Government, due to e.g. an audit.
    * Inventory management software/teams
    * ... could be lots more answers here ...

* **From the perspective of the database itself, is there any difference between a web server, data analyst, or web analytics platform?**
    * NO!!
    * The way they all interact with the database is by providing it with queries. There is a standard API interface and under the hood it all works the same way.