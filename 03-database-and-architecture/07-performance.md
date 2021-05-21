# Performance

In this section we'll discuss some of the main factors impacting performance of both Databases and Database Servers at a high level.

## Factors Impacting Performance

* This is not a comprehensive list, but it lays out some critical factors.

* **What are some factors impacting database performance?**
    * Students will give many answers, some of the critical ones are listed below, but be flexible and explore the ideas students come up with!

* Workload:
    * Reads vs writes
    * Big transactions vs small transactions
    * Time of use considerations & predictability
    * Impact on resource contention
    * Can improve by separating OLTP from OLAP databases.

* Hardware and Network constraints
    * Performance characteristics of CPU
        * Faster is better.
        * More cores -> more concurrency -> generally better performance.
    * Performance characteristics of RAM and the hard drive
        * SDD vs HDD
        * Any other processes on the server(s) accessing the HD?
        * Are they big enough?
        * Throughput v latency.
    * Performance characteristics of the network connection
        * How long are connections left open, and how big is the connection pool?
        * What kind of network hardware is used? Cable v fiber v satelite...
        * Single server, geographic distribution, sharded DBs?
        * Throughput v latency (again)

* (In)Effective use of indexes
    * indexes generally speed up reads and slow down writes.
    * But they also take up memory, and can be expensive to rebuild which slows occupies the whole system for a time.
    * Much more on this tomorrow!

* Contention for the same resources.
    * Writes can lock tables, preventing reads on that table. This is contention.
    * The more uses a single database serves, the more contention there is likely to be.
    * We can reduce contention by paying attention to usage patterns, running dumps and backups at off-peak times, and managing automated scripts and queries similary.

## Performance Metrics

* **Whats the difference between throughput and latency?**
    * Latency is the round trip speed to hit the server and get a response.
        * Latency puts a lower bound on the response time, you can't get a response any faster than the latency to your server.
        * High latency means slow response time.
    * Throughput is the amount of data per unit time that can be sent from the server to a client.
        * High throughput means lots of data per unit time.

    * In an infinite resource situation you'd like to have great latency and great throughput, but sometimes you have to make sacrifices!

* Some useful examples:
    * HD Video Streaming requires high throughput but the latency is kind of irrelevant: If it takes 5 seconds for the video to start streaming, but after that you have high throughput then you'll never have to stop and buffer.
        * Video streaming is like one query that's going to return a LOT of data.

    * Multiplayer video games require low latency, but throughput is less important.
        * The data being sent by each player is generally pretty small. Positional data, or the fact that a key was pressed.
        * High latency --> lag between the player taking an action, and that action being reflected on the server.
        * This is more like an transaction processing workload: many POS systems are constantly sending small updates concurrently.

* Reducing latency can be done by:
    * Reducing the physical distance that a query has to travel, e.g.:
        * Geographic distribution of databases.
        * Colocation of web servers and their respective databases.
    * Upgrading storage to solid state drives.
    * Upgrading network infrastructure (e.g. Fiber Optic)
    * Improving query speed (a fast query takes less time and therefore generates a faster reply)

* Increasing throughput can be done by:
    * Upgrading hardware again, better network cards, better hard drives...
    * Batching queries whenever possible.