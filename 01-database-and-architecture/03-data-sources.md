# Data Sources

In this section we're discussing the many possible sources of data that can contribute to a single Database. We're discussing things in two parts: 

1. Naming the sources and building a diagram of the interconnected systems.
2. A short discussion of ETL and how it can fit into the bigger system.

## Name & Diagram the Sources

* **As you work through this section, maintain a working diagram on the whiteboard of datasources. Add each one individually and talk about it as you do.**
    * Start the diagram with a database server at the center.
        * "Lets pretend for a second this server represents ALL the databases at company..."
    * Let the students lead as much as possible.
    * But throw them a rope if they get stuck.

* **What are the primary sources of data at (insert company name) that ultimately get stored into these databases?**
    * Tons of answers, here are some you should expect:
    * Point of Sales systems.
    * Phone/Web application.
    * Internal reporting/hr systems.
    * Manual upload by analysts.
    * Data provided by external suppliers.
    * Data purchased from a data broker by the firm.

* **For each named datasource, here are some follow up questions to ask**
    * Can the insertion process be automated? To what extent?
    * Is this a recurring process?
        * What systems might mediate that then? Manual trigger/automated?
    * Is this a "streaming" data source or is data batched and dumped all at once?
    * Would the data need to be transformed in some way before it can be stored?

## ETL

* ETL stands for Extract/Transform/Load
* **How does ETL fit into the diagram we've built?**
    * Many sources — in particular data coming from 3rd party systems such as a POS, external vendor, or dump purchased from a data broker — need to be processed before they fit into your schema.
    * First, the relevant information is "extracted"
    * Then, that information is transformed until it fits into your schema.
    * Then, it is loaded into your database.

 * ETL can be one time, or recurring. 
    * You'll develop a system that automatically ETLs data from your POS system directly into your databases.
    * You'll perform a one-time ETL on data purchased from a data broker.
    * You might ETL from an OLTP to OLAP system
        * Online Transaction Processing is a format optimized for writing data and making new records.
        * Online Analysis Processing is a format optimized for querying/reading the data. A common pattern: a nightly ETL process will transform today's new data from OLTP to OLAP.

* **Does anyone have any experience building out ETL systems**
    * If not, tell one horror story of your own. 