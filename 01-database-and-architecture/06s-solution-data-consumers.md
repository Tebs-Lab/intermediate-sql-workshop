# Exercise: Data Consumers

In your groups, discuss the following questions related to the consumers of data for each of the companies.

1. Who are the most important consumers of data?
    1. Consider both human and automatic data consumers.
    2. Are the most important consumers internal or external to the company?
2. What mechanisms might be available for the users to get this data?
    1. How will these mechanisms differ from consumer to consumer?
    2. How will these mechanisms connect with the database?

### Starbucks 

* Most important consumers
    * Analysts, who use the information to make important decisions.
        * Therefore, the set of tools that the analysts use.
        * Including folks who're doing inventory management, logistics, and all kinds of analytics from across the business.
    * The web/phone applications, which mediate customer experience directly.
    * Both are internal.
* Analysts:
    * Direct SQL access via Toad/terminal/PGAdmin.
    * Mediated access via Tableau/Spotfire/PowerBI.
* Apps
    * All apps would be mediated access via a web server.
    * That web server has it's own credentials and may well be co-located with a copy of the relevant DB for it's needs.

### Twitter

* Most important consumers:
    * The users (external).
    * The algorithm for distributing tweets (internal).
    * Analysts (internal).
* Mechanisms:
    * Users are mediated by the app which is mediated by a webserver.
    * The algorithm probably has direct access
        * But probably also involves a large data pipeline separate from the main database for ML Training purposes.
    * Analysts again will have direct and mediated access via terminal/Toad/PGAdmin as well as 3rd party analytics tools.

### Kaiser Permanente

* Most important consumers:
    * Doctors/Nurses and other care providers.
    * Patients are probably a distant second (sadly).
* Mechanisms:
    * Care providers mediated by internal software, almost surely not via SQL directly.
    * Patients via a web app, mediated by a webserver