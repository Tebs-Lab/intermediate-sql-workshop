# Exercise: Security

## Part 1: Discussion

In your groups, discuss the following questions related to the consumers of data for each of the companies.

1. Does this company store sensitive data about individual users?
    1. What data is most sensitive in this regard?
    2. What might the impact of a data breach be in this regard?
    3. Are there legal requirements that govern any of the data?
2. Does the company store data that it considers a company secret, or otherwise privileged?
    1. What data is most sensitive in this regard?
    2. What might the impact of a data breach be in this regard?
    3. Are there legal requirements that govern any of the data?
3. Considering the "principle of least privilege" and previous discussions about data providers and consumers:
    1. How might these consumers and providers be limited in terms of their database access?
    2. To what extent could any of the consumers or providers create an attack vector?

## Reminder: The Companies:

### Starbucks 

Starbucks is an international coffee retailer. They have stores that serve beverages and food, as well as deals to sell their coffee and other products in 3rd party retailers. They also have a large corporate organization, phone and web applications for ordering and customer support, a complex supply chain, and more.

### Twitter

Twitter is a social media company that primarily focuses on short text, photo, and video posts. Posts must be distributed to users around the world in real time. Often a single tweet must be shown to millions of users. Similarly, users are constantly making new tweets, which must all be stored, processed, and distributed to said user's followers.

### Kaiser Permanente

Kaiser Permanente is a large medical company. They provide direct care as well as supporting functions such as medical insurance. They have hospitals in several states — meaning they must acquiesce to many different regulatory regimes (including the federal government). The support a variety of online/tele-health services, as well as support for (somewhat) inter-operative medical records.

## Part 2: Technical

On your copy of the AdventureWorks database make two new users and assign them with appropriate read and write access such that:

1. One of the users represents a sales analyst role.
    1. This person should be able to read product, purchasing, and sales information.
    2. But, this person should not be allowed to access any HR information.
    3. Nor should they be able to write to any table, or create any tables.

2. One of the users represents a program that vendors use to update their own product information.
    1. This user should only be able to write to the Product table and do absolutely nothing else.

Once you've created these users, discuss their specific limitations and the benefits or drawbacks of them. Consider the "principle of least privilege," but also consider the practical impact of these roles for the groups ostensibly using them.