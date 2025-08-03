---
meta:
  title: "Creating transactions: MySQL and PostgreSQL"
  description: "Learn how to create secure transactions in MySQL and PostgreSQL databases to protect your funds and data. Discover the importance of COMMIT and ROLLBACK commands to manage changes and ensure data stability. Explore the use of savepoints for flexible control over transactions, minimizing risks and enhancing data processing efficiency."
---

# Creating transactions

If you attempt to transfer $1,000 from your savings to your checking account and suddenly find that the money has been debited but not credited to your checking account, you would likely be upset. 😿

To protect against such errors, the program processing your transfer request begins a transaction, executes the necessary SQL
queries to transfer money from one account to another, and, if all goes well, completes the transaction by executing the
`COMMIT` command to finalize the changes.

However, if any problems arise, the `ROLLBACK` command is executed, which instructs the server to undo all actions taken since the start of the transaction.

<MySQLOnly>

The process might look like this:

```sql
-- Start the transaction
START TRANSACTION;

-- Check the sender's balance
SELECT @balance := user_balance FROM accounts WHERE user_id = 1;

-- If insufficient funds, cancel the transaction
IF @balance < 1000 THEN
ROLLBACK;
END IF;

-- Check for the existence of the recipient
SELECT @exists := COUNT(*) FROM accounts WHERE user_id = 2;
IF @exists = 0 THEN
ROLLBACK;
END IF;

-- Update account balances if all checks pass
UPDATE accounts SET user_balance = user_balance - 1000 WHERE user_id = 1;
UPDATE accounts SET user_balance = user_balance + 1000 WHERE user_id = 2;

-- Apply changes
COMMIT;
```

</MySQLOnly>

<PostgreSQLOnly>

The process might look like this:

```sql
-- Start the transaction
BEGIN;

-- Check balance using WHERE condition
-- Debit money only if balance is sufficient
UPDATE accounts
SET user_balance = user_balance - 1000
WHERE user_id = 1
  AND user_balance >= 1000;

-- Check that the operation was successful
-- In real applications, the number of updated rows is checked
-- If 0 rows updated - insufficient funds, ROLLBACK needed

-- Credit money to recipient (if they exist)
UPDATE accounts
SET user_balance = user_balance + 1000
WHERE user_id = 2;

-- Apply changes (or ROLLBACK on application error)
COMMIT;
```

</PostgreSQLOnly>

With a transaction, the program ensures the safety of your $1,000, guaranteeing that they either stay in the original account or are transferred to another account, eliminating the risk of loss.

## Starting and completing transactions

<MySQLOnly>

Every explicit transaction in MySQL begins with the use of the `START TRANSACTION` or `BEGIN` statement.

</MySQLOnly>

<PostgreSQLOnly>

Every explicit transaction in PostgreSQL begins with the use of the `BEGIN` or `START TRANSACTION` statement.

</PostgreSQLOnly>

A transaction can be completed by:

- Using the `COMMIT` command, which instructs the server to mark the changes as permanent and release all resources (e.g., row locks) used during the transaction
- Using the `ROLLBACK` command, which requires the server to revert the data to the state before the transaction started. After completing the rollback, any resources used by the transaction are also released.

Besides using `COMMIT` and `ROLLBACK`, a transaction can also end due to external factors.
For example, if the server shuts down, in this case, your transaction will be automatically canceled upon server restart.

## Transaction savepoints

In certain situations, you may need to perform a rollback within a transaction without canceling all the work done.
For this, you can set one or several savepoints within the transaction.
This allows you to roll back to a specific point in the transaction, rather than its start.

Each savepoint within a transaction needs a unique name, which allows for the use of multiple different savepoints.
To create a savepoint named `my_savepoint`, use the following command:

```sql
SAVEPOINT my_savepoint;
```

To roll back to a specific savepoint, simply enter the command `ROLLBACK`, followed by the keywords `TO SAVEPOINT` and the name of the savepoint,
for example:

<MySQLOnly>

```sql
START TRANSACTION;

-- Create a savepoint before changing the balance of the first user
SAVEPOINT before_updating_user_1;
UPDATE accounts SET balance = balance + 100 WHERE user_id = 1;

-- Check condition for the first user
-- for example, we check the logic of business rules

-- Here we assume the condition failed, and we need to cancel the balance change
ROLLBACK TO SAVEPOINT before_updating_user_1;

-- Update the balance for the second user
UPDATE accounts SET balance = balance + 200 WHERE user_id = 2;

-- Complete the transaction
COMMIT;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
BEGIN;

-- Create a savepoint before changing the balance of the first user
SAVEPOINT before_updating_user_1;
UPDATE accounts SET balance = balance + 100 WHERE user_id = 1;

-- Check condition for the first user
-- for example, we check the logic of business rules

-- Here we assume the condition failed, and we need to cancel the balance change
ROLLBACK TO SAVEPOINT before_updating_user_1;

-- Update the balance for the second user
UPDATE accounts SET balance = balance + 200 WHERE user_id = 2;

-- Complete the transaction
COMMIT;
```

</PostgreSQLOnly>

As a result of this transaction, the balance of the first user remains unchanged due to the rollback to the savepoint,
while the balance of the second user increases by 200.
This demonstrates how you can manage changes in the database with a high level of control using transactions and savepoints.

When using savepoints, remember the following points:

- Despite the name, nothing is "saved" when creating a savepoint.
  To make your changes within the transaction permanent, you must execute the `COMMIT` command.
- When performing a transaction rollback without specifying a specific savepoint,
  all previously set savepoints will be ignored, and the entire transaction will be rolled back.
