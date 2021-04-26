# Exercise: Discuss Data Collection

This will be the first of many discussion sections. We will repeatedly discuss the following three companies as archetypes of unique database needs and concerns. The purpose of all these exercises is to help you think about:

* The wide variety of needs and concerns of companies as it relates to data storage.
* The decisions and tradeoffs that database administrators must make with respect to these concerns.
* The impact those decisions have on downstream users of the data.

Each discussion section will focus on the same three companies, but different aspects of their data storage needs. 

## The Companies:

### Starbucks 

Starbucks is an international coffee retailer. They have stores that serve beverages and food, as well as deals to sell their coffee and other products in 3rd party retailers. They also have a large corporate organization, phone and web applications for ordering and customer support, a complex supply chain, and more.

### Twitter

Twitter is a social media company that primarily focuses on short text, photo, and video posts. Posts must be distributed to users around the world in real time. Often a single tweet must be shown to millions of users. Similarly, users are constantly making new tweets, which must all be stored, processed, and distributed to said user's followers.

### Kaiser Permanente

Kaiser Permanente is a large medical company. They provide direct care as well as supporting functions such as medical insurance. They have hospitals in several states — meaning they must acquiesce to many different regulatory regimes (including the federal government). The support a variety of online/tele-health services, as well as support for (somewhat) inter-operative medical records.

## The Exercise, Discussion

In your groups, discuss the following questions for each of the three companies. You may wish to draw diagrams and/or do a bit of searching on Google as you discuss.

1. What are the most important sources of data for this company?
    1. Consider manual and automated processes.
    2. Are these sources tightly controlled by the company?
2. How might this data get processed and stored?
    1. Is the data clean, accurate, and in the proper format when it arrives?
    2. Is other software needed to translate the raw data into a SQL compatible format?
3. If you were to administer these databases, what challenges would you expect with respect to the sources of data and the potential ETL processes?

## The Exercise, Technical

In the data folder of this repo you'll find a file called `sample-import.xlsx`. It is a mock spreadsheet that AdventureWorks has "received" from one of its vendors. The spreadsheet represents  updates to this vendor's min and max order quantities.

Devise and enact a strategy to import this data such that it properly updates rows in the ProductVendor table of the AdventureWorks database. In devising a strategy consider the following...

1. Can you completely automate this?
2. Can you partially automate this, or reduce the number of times you have to initiate a database query to 1?
3. Can you do this without manually (and individually) updating each relevant row in the database?

### Bonus Points

Assume you could impose arbitrary constraints on the vendor in question with regards to the format of this spreadsheet and the information contained therein. What would you have the vendor change and include in order to make this process more (fully) automated?