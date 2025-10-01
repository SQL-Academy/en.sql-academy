---
meta:
    title: 'Stored Functions in SQL'
    description: 'Creating and using stored functions in SQL. Syntax, parameters, return types, and practical examples.'
---

# Stored Functions

Stored functions are a powerful SQL tool that allows you to create reusable code blocks for performing calculations and data transformations. Unlike built-in functions, stored functions are created by developers to solve specific tasks.

> **A stored function** is a named block of SQL code that accepts parameters, performs calculations, and always returns a single value of a specific type.

## General Structure of a Stored Function

<MySQLOnly>

```sql
CREATE FUNCTION function_name(parameter1 TYPE, parameter2 TYPE, ...)
RETURNS return_type
BEGIN
    -- function logic
    RETURN calculation_result;
END;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
CREATE OR REPLACE FUNCTION function_name(parameter1 TYPE, parameter2 TYPE, ...)
RETURNS return_type
LANGUAGE plpgsql
AS $$
BEGIN
    -- function logic
    RETURN calculation_result;
END;
$$;
```

`LANGUAGE plpgsql` — specifies that the function is written in **PL/pgSQL** (PostgreSQL's procedural language).

`AS $$ ... $$` — **dollar quoting**, a special way to delimit the function body. Allows you to avoid escaping characters inside the function.

</PostgreSQLOnly>

## Simple Function Example

Let's create a function to determine if a person is an adult based on their birth date:

<MySQLOnly>

```sql-executable
CREATE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) >= 18;
END;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
CREATE OR REPLACE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN EXTRACT(YEAR FROM AGE(CURRENT_DATE, birth_date)) >= 18;
END;
$$;
```

</PostgreSQLOnly>

Now this function can be used in any query:

<MySQLOnly>

```sql-executable
-- Create the function
CREATE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) >= 18;
END;

-- Use the function
SELECT
    is_adult('2010-05-15') AS child_status,
    is_adult('2000-03-20') AS adult_status;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
-- Create the function
CREATE OR REPLACE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN EXTRACT(YEAR FROM AGE(CURRENT_DATE, birth_date)) >= 18;
END;
$$;

-- Use the function
SELECT
    is_adult('2010-05-15') AS child_status,
    is_adult('2000-03-20') AS adult_status;
```

</PostgreSQLOnly>


## Using Functions in Table Queries

Stored functions are especially useful when working with real data. For example, we can use our function to filter students by age:

<MySQLOnly>

```sql-executable-Schedule
-- Create the function
CREATE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) >= 18;
END;

-- Use the function in a table query
SELECT
    first_name,
    last_name,
    birthday,
    is_adult(birthday) AS is_adult
FROM Student
WHERE is_adult(birthday) = TRUE
LIMIT 5;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Schedule
-- Create the function
CREATE OR REPLACE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN EXTRACT(YEAR FROM AGE(CURRENT_DATE, birth_date)) >= 18;
END;
$$;

-- Use the function in a table query
SELECT
    first_name,
    last_name,
    birthday,
    is_adult(birthday) AS is_adult
FROM Student
WHERE is_adult(birthday) = TRUE
LIMIT 5;
```

</PostgreSQLOnly>

## Functions with Database Queries

Stored functions can execute SQL queries inside themselves to retrieve necessary data:

<MySQLOnly>

```sql-executable-Schedule
CREATE FUNCTION get_student_lessons_count(student_id INT, target_date DATE)
RETURNS INT
BEGIN
    DECLARE lessons_count INT;

    SELECT COUNT(*) INTO lessons_count
    FROM Schedule s
    INNER JOIN Student_in_class sic ON s.class = sic.class
    WHERE sic.student = student_id
      AND s.date = target_date;

    RETURN lessons_count;
END;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Schedule
CREATE OR REPLACE FUNCTION get_student_lessons_count(student_id INT, target_date DATE)
RETURNS INT
LANGUAGE plpgsql
AS $$
DECLARE
    lessons_count INT;
BEGIN
    SELECT COUNT(*) INTO lessons_count
    FROM Schedule s
    INNER JOIN Student_in_class sic ON s.class = sic.class
    WHERE sic.student = student_id
      AND s.date = target_date;

    RETURN lessons_count;
END;
$$;
```

</PostgreSQLOnly>

This function counts the number of lessons for a specific student on a given day:

<MySQLOnly>

```sql
SELECT get_student_lessons_count(1, '2019-09-01') AS lessons_today;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
SELECT get_student_lessons_count(1, '2019-09-01') AS lessons_today;
```

</PostgreSQLOnly>


## Breaking Down the Example with Variables

Let's analyze the previous example step by step to understand how variables and the `INTO` construct work:

```sql
DECLARE lessons_count INT;
```

This line **declares a variable** `lessons_count` of type `INT`. The variable will store the result of our query.

<PostgreSQLOnly>

> **Important for PostgreSQL:** All variables must be declared in the `DECLARE` block before the function body starts (before `BEGIN`). You cannot declare variables inside the function body.

</PostgreSQLOnly>

```sql
SELECT COUNT(*) INTO lessons_count
FROM Schedule s
INNER JOIN Student_in_class sic ON s.class = sic.class
WHERE sic.student = student_id
  AND s.date = target_date;
```

Here we **save the query result into a variable**:

-   `SELECT COUNT(*)` — counts the number of records
-   `INTO lessons_count` — saves the result into the `lessons_count` variable
-   The rest — a regular SQL query with JOIN and conditions

```sql
RETURN lessons_count;
```

**Return the variable value** as the function result.

> **Important:** The `INTO` construct allows you to save the result of a SELECT query into a variable. This is the foundation of working with data inside stored functions.

## Managing Stored Functions

-   **Viewing Existing Functions**

    <MySQLOnly>

    ```sql
    SHOW FUNCTION STATUS WHERE Db = 'your_database_name';
    ```

    </MySQLOnly>

    <PostgreSQLOnly>

    ```sql
    SELECT routine_name, routine_type
    FROM information_schema.routines
    WHERE routine_type = 'FUNCTION' AND routine_schema = 'public';
    ```

    </PostgreSQLOnly>

-   **Dropping a Function**

    <MySQLOnly>

    ```sql
    DROP FUNCTION IF EXISTS is_adult;
    ```

    </MySQLOnly>

    <PostgreSQLOnly>

    ```sql
    DROP FUNCTION IF EXISTS is_adult(DATE);
    ```

    </PostgreSQLOnly>

-   **Modifying a Function**

    <MySQLOnly>

    To modify a function in MySQL, you need to drop the old version first, then create a new one:

    ```sql
    DROP FUNCTION IF EXISTS is_adult;
    -- Create new version of the function
    CREATE FUNCTION is_adult(birth_date DATE) ...
    ```

    </MySQLOnly>

    <PostgreSQLOnly>

    In PostgreSQL, you can use `CREATE OR REPLACE FUNCTION`:

    ```sql
    CREATE OR REPLACE FUNCTION is_adult(birth_date DATE)
    RETURNS BOOLEAN
    -- new implementation
    ```

    </PostgreSQLOnly>

Stored functions are a powerful tool for creating reusable business logic directly in the database. They help centralize calculations and ensure data consistency across the entire application! 🚀
