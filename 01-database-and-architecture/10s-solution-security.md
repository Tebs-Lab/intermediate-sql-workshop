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

## The Companies:

### Starbucks 

* Login credentials are always sensitive.
    * Lateral movement if user has bad security hygiene (they do).
    * Make fraudulent purchase via the app possibly
* Credit card info potentially, which does have legal implications.
    * Stolen card info can lead to a broader range of fraudulent transactions
* Logistics and recipe information might be sensitive
* Employment info may well contain SS numbers and other sensitive info
* Least Privilege:
    * Analysts can be limited to their specific purview, and to read only.
        * Could be allowed to create views and even indexes in some cases.
    * App server should have it's own user and limited explicitly to the known interactions that the server can utilize.

### Twitter

* Login credentials are always sensitive.
    * Risks are fairly low for Twitter, but moreso for high profile accounts.
        * Takeover of POTUS vs takeover of random user w/ 10 followers.
* Most posts are public, although private posts might be considered sensitive information.
* Again internal HR type data might be more sensitive.
* Again, potential credit card info for paying folks.
* Least Privilege:
    * The app server is really the main role, and it needs read/write to all the app data.
    * Completely separate the HR database and the app...

### Kaiser Permanente

* Login credentials are always sensitive.
    * And a fraudulent login exposes sensitive patient information!
* Medical data is one of the most sensitive kinds of all data.
    * And does have legal requirements governing it.
* Different systems should have very different access...
    * Consumer side login app vs doctor apps.
* Definitely worth encrypting this data on the servers and in transit.
* Least Privilege:
    * Extremely limited permissions for the consumer facing apps.
    * Doctor facing apps probably need more access.
    * Ad hoc analysts etc must be tightly controlled as well, to comply with laws.

## Part 2: Technical

On your copy of the AdventureWorks database make two new users and assign them with appropriate read and write access such that:

1. One of the users represents a sales analyst role.
    1. This person should be able to read product, purchasing, and sales information.
    2. But, this person should not be allowed to access any HR information.
    3. Nor should they be able to write to any table, or create any tables.

```
create role analyst login password 'data!';
grant usage on schema production, purchasing, sales to analyst;
grant select on all tables in schema production, purchasing, sales to analyst;
```

2. One of the users represents a program that vendors use to update their own product information.
    1. This user should only be able to write to the Product table and do absolutely nothing else.

```
create role vendor_app login password 'vending';
grant usage on schema production to vendor_app;
grant update, insert on production.Product to vendor_app; 
```

Once you've created these users, discuss their specific limitations and the benefits or drawbacks of them. Consider the "principle of least privilege," but also consider the practical impact of these roles for the groups ostensibly using them.

* Tons of thing could be relevantly discussed!
    * the analyst role might be too broad, does one analyst really need access to all those tables? Some surely need it, but giving it to ALL analysts might be a small security risk
        * But it sure is convenient... 
    * Single table access is great, but should I have given update and insert or just update?
        * Depends on the actual app being exposed to vendors.
    * Maintaining TONS of roles can also be a security risk, as managing them becomes unwieldy and they could be come out of date.