# Schema Design

The design of a schema can have a major impact on the overall performance and usefulness of a database. Here are some of the most important considerations:

## Evaluate the Relationships

* Getting 1-to-1, 1-to-many, and many-to-many relationships right from the start is reallllly helpful.
* join tables and complex relationships make databases more complex, but stuffing tons of values (especially duplicate values) into a single table also creates issues.
* find the balance!

## What Fields Should Be Required?

* Enforcing constraints can be incredibly useful.
    * If a value should never be missing, make it non-null.
    * If a value should always be a reference to a field in another table, make it an FK constraint.

## Data Duplication & Level of Normalization

* Storing each actual value the fewest number of times can save space. But sometimes this isn't reasonable.
    * Consider names in a user table. Some of your users will have the same name. Should you really put all the names into a separate table and use FK relationships in the user table to identify unique names?
    * To completely deduplicate your data will often make the database harder to query and use.

* Normalization in part refers to this deduplication, but there are other rules for 1st/2nd/3rd... normal forms
    * [https://www.guru99.com/database-normalization.html](https://www.guru99.com/database-normalization.html)
    * The specific rules are really boring, but here is the gist:
    * The more Normalized a database is, the less chance there is for anomalies during insert/update/delete
        * Highly denormalized databases might have a list of checked out books for a single person in a single row of the DB.
            * A more normalized schema would have a books table and a users table, with a checked_out_id that is an FK to the patrons table.

## Silo'd Security

* Highly sensitive information that is logically connected to other non-sensitive data should use a one-to-one relationship to enable better "principle of least privilege" access. 
    * For example, users might supply a SSN.
    * Make a table for the sensitive info, and give it an FK back to the user. Now you can limit access to that data.

## Understand Usage Patterns

* Is this OLTP or OLAP?
    * In OLTP you may want to index many fewer tables/columns.
* Start with indexes on fields you know will be queried frequently.
