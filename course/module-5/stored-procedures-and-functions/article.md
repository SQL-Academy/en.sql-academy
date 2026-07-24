---
meta:
    title: "Stored Procedures and Functions in SQL"
    description: "Introduction to SQL stored procedures and functions. Their purpose, key differences, and when to use each type."
---

![Article banner](https://sql-academy.org/static/guidePage/stored-procedures-and-functions/banner.jpg)# Stored Procedures and FunctionsIn SQL, beyond regular queries, there are more powerful tools — **stored procedures** and **stored functions**.
These objects allow you to create ready-made code blocks that can be used repeatedly 🔄.> **Stored procedures and functions** are pre-written SQL scripts saved in the database that can be called by name.Imagine you have a complex query for calculating sales statistics that you use every day.
Instead of rewriting it each time, you can create a procedure or function and simply call it!## Why do we need them?Stored procedures and functions solve several important tasks:\* **🚀 Code Reusability** — write once, use everywhere. No more copies of the same code in different places.

- **⚡ Performance** — code executes directly on the database server, which is often faster than regular queries.
- **🔒 Security** — you can grant access to a procedure without giving direct access to tables.
- | **🛡️ Centralized Logic** — all business logic is in one place, in the database.## Key DifferencesAlthough procedures and functions are similar, there are important differences between them:**MySQL** | Characteristic                           | Stored Procedures                                        | Stored Functions                                                                                                                                                         |
  | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- | ----------------- | ---------------- |
  | **Return Value**                                                                                                                                                                                       | May not return or return multiple values | Always returns a single value                            |
  | **Usage in Queries**                                                                                                                                                                                   | Called separately                        | Can be used in SELECT, WHERE, and other parts of queries |
  | **Data Modification**                                                                                                                                                                                  | Can modify data in tables                | Designed only for reading data                           |
  | **Invocation**                                                                                                                                                                                         | `CALL procedure_name()`                  | `SELECT function_name()`                                 | **PostgreSQL**                                                                                                                                                           | Characteristic | Stored Procedures | Stored Functions |
  | ---------------------                                                                                                                                                                                  | -------------------------                | -------------------------------------------------------- |
  | **Return Value**                                                                                                                                                                                       | Cannot return values                     | Always returns a single value                            |
  | **Usage in Queries**                                                                                                                                                                                   | Called separately                        | Can be used in SELECT, WHERE, and other parts of queries |
  | **Data Modification**                                                                                                                                                                                  | Can modify data in tables                | Can modify data in tables                                |
  | **Invocation**                                                                                                                                                                                         | `CALL procedure_name()`                  | `SELECT function_name()`                                 | ## When to use procedures?**Stored procedures** work best when you need to:\* Execute a sequence of operations (e.g., create order, deduct inventory, send notification) |
- Modify data in multiple tables simultaneously
- Implement complex business logic
- Return multiple result sets### Procedure Usage ExampleLet's say we need to create a procedure for order processing:```sql
  -- Example procedure call (conceptual)
  CALL create_order(customer_id = 123, product_id = 456, quantity = 2);

````Such a procedure can:1) Check product availability in stock
2) Create a record in the orders table
3) Update product inventory
4) Add a record to operation history## When to use functions?**Stored functions** are ideal when you need to:* Perform calculations and return a result
* Create a reusable formula
* Transform data in a specific way
* Use the result in other SQL queries### Function Usage ExampleLet's create a function for discount calculation:```sql
-- Example of using function in a query
SELECT
    product_name,
    price,
    calculate_discount(price, customer_type) AS discount_amount
FROM Products;
```Such a function takes price and customer type, and returns the discount amount that can be used in any queries.## Simple Selection RuleIf you're unsure what to choose, use this simple rule:* **Need to get a single value for use in a query?** → Function
* **Need to execute a set of actions or modify data?** → Procedure## Reinforce Your KnowledgeNow that you know the key differences between procedures and functions, try classifying tasks in this interactive game:The interactive demonstration is available [in the SQL Academy lesson](https://sql-academy.org/en/guide/stored-procedures-and-functions).## What's Next?This was an introductory article to understand the general concept. In the following materials, we'll cover in detail how to create and work with stored procedures and functions.
````
