# Exercise: Performance Considerations

In your groups, discuss the following questions related to the consumers of data for each of the companies.

1. Define this company's workload with respect to reads...
    1. Who/what is querying the data?
    2. What kinds of queries are likely to be most common?
    3. Will the reads be fetching lots of data at once?
    4. Will there be lots of reads happening all the time?
2. Define this company's workload with respect to writes...
    1. Who/what is storing data?
    2. How often will data need to be stored?
    3. What size of data will be stored in an individual connection/query?
3. How much latency is acceptable?
4. How much bandwidth is required?

## Reminder: The Companies:

### Starbucks 

* Reads are primarily from two (three?) sources:
    * The analysts making ad hoc and predefined report queries.
        * Less predictable. Many will be ad hoc explorations of the data. 
        * Mediated by 3rd party software (tableau/spotfire/powerBI)
        * Mix of large and small queries.
        * Mostly happening during business hours (but with a global business what does that mean?)
    * The phone/web app servers, to serve requests.
        * Small, predictable, well understood queries related to the customers using the apps.
    * The machine learning people?
        * They will occasionally request a huge amount of data.
        * But they likely have a lot of their own bespoke infrastructure to support these workloads
* Writes will be happening constantly.
    * Likely many small writes (e.g. one per customer transaction)
    * POS systems, the web app, and perhaps 3rd party integrations like Stripe for web pay might all be sending data to the DB.
* Because of how often reads and writes both happen, might be wise to separate the OLTP from OLAP datastores.
    * Big nightly ETL for today's sales into analysis DB.
* Latency for each POS transaction is important, customers are waiting for those transactions to complete before they have paid. Has an impact on partners in the stores and customer satisfaction
* Overall throughput is probably not a HUGE issue for the writes.
    * Each transaction is small, and in an individual store spaced by at least several seconds, even during peak business hours. 
    * But, concurrency/contention is a potential issue. Clustering/Sharding the DBs by store id could improve this.
* Throughput for the analytics people might matter more, depending on the kinds of queries being made. 

### Twitter

* Billions of reads per day.
    * Reads are constant, but also spiky: when some news breaks or some situation is developing the servers get hammered.
    * The query pattern is very predictable — since it's basically always mediated by Twitter built software — and therefore highly optimizable.
    * Each query is probably small, single tweets or a small batch of tweets.
* Writes are less common than reads — each tweet is broadcast to several users feeds — but still very common and happening ALL THE TIME!
    * Because tweets can never be edited, the amount of contention is reduced. There's no real need to lock tables when adding tweets, that user will just get them next time they query.
    * Most writes are very small: 280 chars of text.
        * But video and photo content challenges that assumption.
    * Twitter is a "real time" platform, so latency is critical.
    * Throughput is also really important just because of the sheer volume of users requesting data alllllllllll the time.
* It's worth noting that Twitter has [literally invented custom DBMS systems](https://blog.yugabyte.com/recap-the-distributed-database-behind-twitter/) to [support their workloads](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2017/the-infrastructure-behind-twitter-scale.html)

### Kaiser Permanente

* Mostly doctors and patients (both mediated by software) making queries.
    * Occasionally.
* It's exactly the same for writes.
* Honestly, I doubt Kaiser has many high performance database concerns. 
* Boring! 