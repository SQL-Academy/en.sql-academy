---
meta:
    title: "Stored Functions in SQL"
    description: "Creating and using stored functions in SQL. Syntax, parameters, return types, and practical examples."
---

# Stored Functions

Stored functions are a powerful SQL tool that allows you to create reusable code blocks for performing calculations and data transformations. Unlike built-in functions, stored functions are created by developers to solve specific tasks.

> **A stored function** is a named block of SQL code that accepts parameters, performs calculations, and always returns a single value of a specific type.

## General Structure of a Stored Function

**MySQL**

```sql
CREATE FUNCTION function_name(parameter1 TYPE, parameter2 TYPE, ...)
RETURNS return_type
BEGIN
    -- function logic
    RETURN calculation_result;
END;
```

**PostgreSQL**

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

## Simple Function Example

Let's create a function to determine if a person is an adult based on their birth date:

**MySQL**

```sql
CREATE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) >= 18;
END;
```

**PostgreSQL**

```sql
CREATE OR REPLACE FUNCTION is_adult(birth_date DATE)
RETURNS BOOLEAN
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN EXTRACT(YEAR FROM AGE(CURRENT_DATE, birth_date)) >= 18;
END;
$$;
```

Now this function can be used in any query:

**MySQL**

```sql
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

**PostgreSQL**

```sql
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

**MySQL**

| child_status | adult_status |
| ------------ | ------------ |
| 0            | 1            |

**PostgreSQL**

| child_status | adult_status |
| ------------ | ------------ |
| false        | true         |

## Using Functions in Table Queries

Stored functions are especially useful when working with real data. For example, we can use our function to filter students by age:

**MySQL**

```sql
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

**PostgreSQL**

```sql
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

**MySQL**

| first_name | last_name | birthday                 | is_adult |
| ---------- | --------- | ------------------------ | -------- |
| Nikolaj    | Sokolov   | 2000-10-01T00:00:00.000Z | 1        |
| Vyacheslav | Eliseev   | 2000-11-21T00:00:00.000Z | 1        |
| Ivan       | Efremov   | 2000-09-19T00:00:00.000Z | 1        |
| Anatolij   | ZHdanov   | 2007-07-15T00:00:00.000Z | 1        |
| Georgij    | Noskov    | 2000-03-03T00:00:00.000Z | 1        |

**PostgreSQL**

| first_name | last_name | birthday                 | is_adult |
| ---------- | --------- | ------------------------ | -------- |
| Nikolaj    | Sokolov   | 2000-10-01T00:00:00.000Z | true     |
| Vyacheslav | Eliseev   | 2000-11-21T00:00:00.000Z | true     |
| Ivan       | Efremov   | 2000-09-19T00:00:00.000Z | true     |
| Anatolij   | ZHdanov   | 2007-07-15T00:00:00.000Z | true     |
| Georgij    | Noskov    | 2000-03-03T00:00:00.000Z | true     |

## Functions with Database Queries

Stored functions can execute SQL queries inside themselves to retrieve necessary data:

**MySQL**

```sql
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

**PostgreSQL**

```sql
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

This function counts the number of lessons for a specific student on a given day:

**MySQL**

```sql
SELECT get_student_lessons_count(1, '2019-09-01') AS lessons_today;
```

**PostgreSQL**

```sql
SELECT get_student_lessons_count(1, '2019-09-01') AS lessons_today;
```

| lessons_today |
| ------------- |
| 3             |

## Breaking Down the Example with Variables

Let's analyze the previous example step by step to understand how variables and the `INTO` construct work:

```sql
DECLARE lessons_count INT;
```

This line **declares a variable** `lessons_count` of type `INT`. The variable will store the result of our query.

**PostgreSQL**

> **Important for PostgreSQL:** All variables must be declared in the `DECLARE` block before the function body starts (before `BEGIN`). You cannot declare variables inside the function body.

```sql
SELECT COUNT(*) INTO lessons_count
FROM Schedule s
INNER JOIN Student_in_class sic ON s.class = sic.class
WHERE sic.student = student_id
  AND s.date = target_date;
```

Here we **save the query result into a variable**:

- `SELECT COUNT(*)` — counts the number of records
- `INTO lessons_count` — saves the result into the `lessons_count` variable
- The rest — a regular SQL query with JOIN and conditions

```sql
RETURN lessons_count;
```

**Return the variable value** as the function result.

> **Important:** The `INTO` construct allows you to save the result of a SELECT query into a variable. This is the foundation of working with data inside stored functions.

## Managing Stored Functions

- **Viewing Existing Functions**

    **MySQL**

    ```sql
    SHOW FUNCTION STATUS WHERE Db = 'your_database_name';
    ```

    **PostgreSQL**

    ```sql
    SELECT routine_name, routine_type
    FROM information_schema.routines
    WHERE routine_type = 'FUNCTION' AND routine_schema = 'public';
    ```

- **Dropping a Function**

    **MySQL**

    ```sql
    DROP FUNCTION IF EXISTS is_adult;
    ```

    **PostgreSQL**

    ```sql
    DROP FUNCTION IF EXISTS is_adult(DATE);
    ```

- **Modifying a Function**

    **MySQL**

    To modify a function in MySQL, you need to drop the old version first, then create a new one:

    ```sql
    DROP FUNCTION IF EXISTS is_adult;
    -- Create new version of the function
    CREATE FUNCTION is_adult(birth_date DATE) ...
    ```

    **PostgreSQL**

    In PostgreSQL, you can use `CREATE OR REPLACE FUNCTION`:

    ```sql
    CREATE OR REPLACE FUNCTION is_adult(birth_date DATE)
    RETURNS BOOLEAN
    -- new implementation
    ```

Stored functions are a powerful tool for creating reusable business logic directly in the database. They help centralize calculations and ensure data consistency across the entire application! 🚀
