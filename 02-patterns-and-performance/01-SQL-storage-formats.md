# How is Data Stored in a SQL Database

Admittedly this section deservers like, half a semester worth of study, but nevertheless a quick overview can be really illuminating with regards to how databases work, how queries are performed, and some of the performance characteristics of databases.

Note that every DBMS does things a little differently. We're talking about Postgres. Conceptually DBMSes work in generally similar ways but some of the details will vary database to database.

The big reason for this section is to demystify some of the magic that databases 

## Some Quick Terminology:

These are words you'll see in the docs, so it's worth calling them out. 

* `tuple` and `item` both mean "row"
* `relation` means "table"
* a `filenode` is an id that references an index of a table.
* a `block` and `page` both mean a configurable sized (default->8kb) segment of data containing information about the `page` as well as data from a table.
    * query current setting: `SELECT current_setting('block_size');`
* `CTID` is short for Column Tuple IDentifier. This represents the physical location of the row version within its table. This is actually available as a column in the database, though it must be explicitly queried:

```sql
AdventureWorks=# select name, CTID from production.product limit 10;
         name          |  ctid  
-----------------------+--------
 Adjustable Race       | (0,1)
 Bearing Ball          | (0,2)
 BB Ball Bearing       | (0,3)
 Headset Ball Bearings | (0,4)
 Blade                 | (0,5)
 LL Crankarm           | (0,6)
 ML Crankarm           | (0,7)
 HL Crankarm           | (0,8)
 Chainring Bolts       | (0,9)
 Chainring Nut         | (0,10)
```

* These numbers inform us that these records from the production.product table are in block 0, and they each have a different index within that block.
    * These block numbers are relative to the relation, and index values are relative to the block, which is why if you do this with another table their blocks may be identical.

* OID stands for Object Identifier.
* A `database cluster` is a collection of databases managed by a single DBMS instance. Databases in the cluster generally share a storage area on disk.
* `VACCUM` is a process that Postgres runs periodically to clean up and optiomize the physical storage.

## So Where Exactly is the Data?

* First, when Postgres is launched it specifies the data directory. 
    * On my machine it's located at: `/Library/PostgreSQL/13/data`
    * You can find it using this query: `show data_directory;`

* Note that generally the `postgres` user owns this directory. Lets go there:
```
sudo su postgres
cd /Library/PostgreSQL/13/data
ls -l

-rw-------    1 postgres  daemon      3 Oct  6  2020 PG_VERSION
drwx------    7 postgres  daemon    224 Apr 26 17:41 base
-rw-------    1 postgres  daemon     44 Apr 28 13:57 current_logfiles
drwx------   60 postgres  daemon   1920 Apr 26 17:44 global
drwxr-xr-x  148 postgres  daemon   4736 Apr 28 13:57 log
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_commit_ts
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_dynshmem
-rw-------    1 postgres  daemon   4566 Oct  6  2020 pg_hba.conf
-rw-------    1 postgres  daemon   1636 Oct  6  2020 pg_ident.conf
drwx------    5 postgres  daemon    160 Apr 28 14:57 pg_logical
drwx------    4 postgres  daemon    128 Oct  6  2020 pg_multixact
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_notify
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_replslot
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_serial
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_snapshots
drwx------    2 postgres  daemon     64 Apr 26 17:08 pg_stat
drwx------    8 postgres  daemon    256 Apr 28 16:23 pg_stat_tmp
drwx------    3 postgres  daemon     96 Oct  6  2020 pg_subtrans
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_tblspc
drwx------    2 postgres  daemon     64 Oct  6  2020 pg_twophase
drwx------   10 postgres  daemon    320 Apr 26 17:49 pg_wal
drwx------    3 postgres  daemon     96 Oct  6  2020 pg_xact
-rw-------    1 postgres  daemon     88 Oct  6  2020 postgresql.auto.conf
-rw-r--r--    1 postgres  daemon  27931 Oct  6  2020 postgresql.conf
-rw-------    1 postgres  daemon     71 Apr 26 17:08 postmaster.opts
-rw-------    1 postgres  daemon     86 Apr 26 17:08 postmaster.pid
```

* Bunch of stuff can be ignored for now, it's a lot of configuration and logs and stuff like that.
* the `base` folder contains the actual data.

```
cd base
ls -l
drwx------  291 postgres  daemon   9312 Jan 19 10:43 1
drwx------  291 postgres  daemon   9312 Oct  6  2020 13708
drwx------  292 postgres  daemon   9344 Apr 26 17:44 13709
drwx------  637 postgres  daemon  20384 Apr 26 17:49 18516
drwx------    2 postgres  daemon     64 Apr 26 17:42 pgsql_tmp
```

* Cool, great file names right? Lets narrow things down. Back in PGAdmin or psql:

```
AdventureWorks=# select oid, datname from pg_database;
  oid  |    datname     
-------+----------------
 13709 | postgres
     1 | template1
 13708 | template0
 18516 | AdventureWorks
(4 rows)
```

* This tells us that all the AdventureWorks data is in `base/18516`

```
cd 18516
ls -l
## HOLY MOLY A TON OF FOLDERS NAMED NUMBERS ##
```

* Lets find a single table... back in psql or PGAdmin

```
AdventureWorks=# select pg_relation_filepath('production.Product');
 pg_relation_filepath 
----------------------
 base/18516/18917
(1 row)
```

* Cool, check it out...

```
xxd 18917 | less # Shows the offests in hex as well as the raw data
## Lots of garbagey looking text... but look for the names of the products!!## 
```

* Everything in here is a binary data storage format, managed explicitly by the PostgreSQL DBMS. 
    * Information is densely packed. Strings are stored raw, numbers might take 16, 32, 64 bits or other sizes depending on the specified format. 
    * Each of these files has a very specific format, including a header that has a lot of information about the file itself, and a bunch of tuples representing the rows.
    * The postgres software knows how to efficiently read and write to this data storage format.

* One more thing to note:

```
# in the folder for the AdventureWorks DB:
ls -l
# Note the sizes of all these files are clean multiples of the block size
-rw-------  1 postgres  daemon      8192 Apr 26 17:41 112
-rw-------  1 postgres  daemon      8192 Apr 26 17:41 113
-rw-------  1 postgres  daemon    155648 Apr 26 17:46 1247
-rw-------  1 postgres  daemon     24576 Apr 26 17:46 1247_fsm
-rw-------  1 postgres  daemon      8192 Apr 26 17:46 1247_vm
-rw-------  1 postgres  daemon    843776 Apr 26 17:46 1249

```

* 155648 / 8192 = 19
* 24576 / 8192 = 3
* 155648 / 8192 = 103