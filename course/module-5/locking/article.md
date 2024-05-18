---
meta:
  title: "Locks in DBMS"
  description: "Understanding locks in DBMS: Discover how different types of locks help manage simultaneous data access and ensure correct transaction processing. Explore the various levels of lock granularity—from tables to rows—and their impact on performance and data consistency in your database."
---

# Locks in DBMS

Database management systems provide the ability for a user to retrieve and modify data.
However, in the modern world, thousands of people can be modifying a database at the same time.
If users mostly perform read operations, such load doesn't pose a significant challenge for the database server.
But if some users simultaneously add or change data, the server faces much more complex tasks.

Suppose you are preparing a financial report summarizing the daily sales of a store over a week.
At the same time you are working on the report, the following operations take place:

- A customer purchases an item
- Another customer returns a defective item and receives a refund
- The store receives a new batch of goods

Thus, during the formation of your report, several users are changing the information in the database.
So what figures should appear in the report? 🧐

The answer depends on how your server handles the lock.

## Locking

Locking is a method of restricting access to data to ensure correct transaction processing.

Database servers use locks to manage simultaneous access to data so that while one transaction is working with the data,
other transactions cannot modify it.

When data in the database is locked, other users who want to change or read the same data must wait until the lock is released.

### Lock Granularity

There are several different strategies that can be used to lock a resource.
The server can apply a lock at one of three different levels, or granularities:

- Table locks  
  Prevent multiple users from simultaneously modifying data in the same table.
- Page locks  
  Prevent multiple users from changing data on the same page (a page is a memory segment, usually ranging from 2 to 16 KB)
  of a table simultaneously.
- Row locks  
  Prevent multiple users from simultaneously changing the same row in the table.

These approaches have their advantages and disadvantages.
Locking an entire table requires minimal time, but as the number of users increases, it can lead to long waits.
Row locking requires more overhead, but it allows multiple users to modify the same table if they are working on different rows.

MySQL can use table, page, or row locking depending on your storage engine choice.
By default, MySQL uses the InnoDB storage engine, which provides row-level locking.

Before moving on to the next article about creating transactions, let's check how well you understood this lesson.
