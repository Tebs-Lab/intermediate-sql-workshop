# Exercise: Design a Schema

In groups you will design a possible schema for a library (as in, a place you can go to rent books, typically for free). Your library must be able to support, at minimum, the following:

1. Record every book in the library, including:
    1. Whether or not that book is checked out, to whom it is checked out, and when it is due back.
    2. Supporting information (who wrote it, publication date, etc.)
    3. Where it belongs in the stack (e.g. Dewey Decimal system)

2. Record every library member, including:
    1. All the books they currently have checked out.
    2. Any fees they owe.

3. A bespoke system that library members could use to search for books by:
    1. Genre
    2. Author
    3. Publication date

4. A bespoke system that library administrators could use to search for books by:
    1. Checked in / Checked out status
    2. Due date  

**Your design should be relatively high level, but consider the following questions explicitly in your design:**  

1. What are the names of all the tables, and what columns would they need?
2. What are the foreign key relationships?
3. How much normalization is appropriate?
    1. For example, is it beneficial to have an Authors table vs AuthorName as a field in the Books table?
4. Given the search requirements, which fields should be indexed?
    1. What kind of indexes might you use?