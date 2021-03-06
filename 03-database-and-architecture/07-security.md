# Security

In this section we'll discuss some common security concerns related to SQL databases and their administration.

## Threat Modeling

* Threat modeling is at the core of any security question, essentially it boils down to these two questions:
    * Who are your potential adversaries?
    * What do they want to do on you systems?

* In the context of databases, the most common asset that needs protecting is sensitive user data.
    * Login credentials,
    * Social security numbers,
    * Credit card info, 
    * Address,
    * ...

* **But what else might be interesting to attackers?**
    * Trade secrets such as recipes, formulas, etc.
        * Covid Vaccine formulas were targeted for example.
    * Private correspondences
    * Write access -> ransomware 
    * Write access can also -> Remote Code Execution if the SQL user can write to a file in some cases.

## Attack Vectors

* Phishing
* SQL Injection via web apps / phone apps
* Unauthorized physical access (or VPN access)

## Mitigation Strategies

* The principle of least privilege:
    * Any SQL user should have the minimum amount of privileges required to do their job.
    * This can mean limiting an analyst to read access.
    * Limiting access to specific tables.
    * Allow an automated process only to write to exactly the table relevant for that process...

* Encrypt the data.
    * This can be in-flight via something like SSL
    * It can also mean on the hard drive. Most SQL implementations these days support encryption.
        * Comes with a cost, encryption takes time and will increase the cost of both reads and writes on the encrypted fields.

* Old software.
    * Out of date software is one of the MOST common attack vectors. 
    * Probably second only to phishing

## Examining Roles

In Postgres you can use `\du` command in the CLI to see the users. With the -E version you can see the query it generates!

```
psql -U postgres -E
postgres=# \du+
********* QUERY **********
SELECT r.rolname, r.rolsuper, r.rolinherit,
  r.rolcreaterole, r.rolcreatedb, r.rolcanlogin,
  r.rolconnlimit, r.rolvaliduntil,
  ARRAY(SELECT b.rolname
        FROM pg_catalog.pg_auth_members m
        JOIN pg_catalog.pg_roles b ON (m.roleid = b.oid)
        WHERE m.member = r.oid) as memberof
, pg_catalog.shobj_description(r.oid, 'pg_authid') AS description
, r.rolreplication
, r.rolbypassrls
FROM pg_catalog.pg_roles r
WHERE r.rolname !~ '^pg_'
ORDER BY 1;
**************************

                                          List of roles
 Role name |                         Attributes                         | Member of | Description 
-----------+------------------------------------------------------------+-----------+-------------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}        | 
```

* We can run the query it in PGAdmin too...
* The \l and \z [table] commands are useful too

```
postgres=# \l

                               List of databases
      Name      |  Owner   | Encoding | Collate | Ctype |   Access privileges   
----------------+----------+----------+---------+-------+-----------------------
 AdventureWorks | postgres | UTF8     | C       | C     | 
 postgres       | postgres | UTF8     | C       | C     | 
 template0      | postgres | UTF8     | C       | C     | =c/postgres          +
                |          |          |         |       | postgres=CTc/postgres
 template1      | postgres | UTF8     | C       | C     | =c/postgres          +
                |          |          |         |       | postgres=CTc/postgres
(4 rows)

postgres=# \z AdventureWorks.production.Product
                            Access privileges
 Schema | Name | Type | Access privileges | Column privileges | Policies 
--------+------+------+-------------------+-------------------+----------
(0 rows)
```

* Lets create a new user, and grant them some specific privileges:

```
# Connect to the AdventureWorks DB first, if you're in CLI
\c AdventureWorks

# Create the role
create role hr_read_only login password 'very safe';

# You can check it exists with \du or the SQL above generated by \du
\du

# Let them acces the schema and read every table in the schema.
grant usage on schema humanresources to hr_read_only;
grant select on all tables in schema humanresources to hr_read_only;

# Login as the new user (change connection info in PG admin or):
\c - hr_read_only

# Works:
select * from humanresources.department;

# Denied!
select * from production.Product;
```

There are LOTS of combinations for permissions... I'm not going to demo them all here but the documentation provides start: [https://www.postgresql.org/docs/13/sql-grant.html](https://www.postgresql.org/docs/13/sql-grant.html)