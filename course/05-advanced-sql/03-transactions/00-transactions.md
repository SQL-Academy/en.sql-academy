---
meta:
    title: "Transactions"
    description: "Discover the importance of transactions in database management and how they ensure the reliability of applications. Learn why transactions are necessary to maintain data integrity and how they help prevent errors during simultaneous access by multiple users."
---

# Transactions

If database servers operated flawlessly 100% of the time,
if users always allowed programs to complete execution, and if applications always concluded without fatal errors, there would be nothing to discuss regarding simultaneous access to the database.

However, such an ideal situation is unrealistic, and therefore we must consider mechanisms
that allow multiple users to work with the same data.
One of the key elements in solving this issue is the transaction.

> A transaction is a sequence of database operations that are executed as a single unit.

In this section, we will discuss transactions that allow combining multiple SQL statements into one group,
ensuring that either all the statements are successfully executed or none of them are executed.

## Test yourself

What does a transaction guarantee?

1. Queries inside a transaction cannot fail — A transaction does not prevent errors. If a query fails, the whole transaction can be undone so that the database does not keep only part of its changes.

2. **Correct answer:** Either all changes of the transaction are saved, or none of them are — A transaction runs its operations as a single unit, so the database never keeps only part of its changes.

3. Other users cannot read the tables until the transaction ends — A transaction does not by itself lock whole tables. How other users work with the data a transaction changes is controlled by locks, the topic of the next lesson.
