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

Starbucks is an international coffee retailer. They have stores that serve beverages and food, as well as deals to sell their coffee and other products in 3rd party retailers. They also have a large corporate organization, phone and web applications for ordering and customer support, a complex supply chain, and more.

### Twitter

Twitter is a social media company that primarily focuses on short text, photo, and video posts. Posts must be distributed to users around the world in real time. Often a single tweet must be shown to millions of users. Similarly, users are constantly making new tweets, which must all be stored, processed, and distributed to said user's followers.

### Kaiser Permanente

Kaiser Permanente is a large medical company. They provide direct care as well as supporting functions such as medical insurance. They have hospitals in several states — meaning they must acquiesce to many different regulatory regimes (including the federal government). The support a variety of online/tele-health services, as well as support for (somewhat) inter-operative medical records.