---
meta:
    title: "IF, CASE, WHILE Operators in Stored Procedures and Functions"
    description: "Learn conditional statements and loops in SQL stored procedures and functions. Syntax and examples of IF, CASE, WHILE for MySQL and PostgreSQL."
---

**MySQL**

# IF, CASE, WHILE Operators in Stored Procedures

**PostgreSQL**

# IF, CASE, WHILE Operators in Stored Functions

Stored procedures and functions are not just convenient containers for groups of queries. They allow you to implement quite complex logic using conditional operators and loops.

In this article, we'll explore the main flow control operators: conditional statements `IF` and `CASE`, as well as `WHILE` loops.

## IF Conditional Statement

The `IF` operator allows you to execute code based on whether a condition is met.

### IF Syntax

**MySQL**

```sql
IF condition THEN
    statements
[ELSEIF condition THEN
    statements] ...
[ELSE
    statements]
END IF;
```

- `condition` — a logical expression, for example `student_age < 18`. The first branch whose condition
  is true runs, the rest are skipped.
- `statements` — one or more SQL statements, each ending with `;`. They run if this branch is chosen.
- `ELSEIF` (keyword, optional) — another branch with its own condition. It's checked only if all the conditions above
  are false. You can add several of these branches.
- `ELSE` (keyword, optional) — the branch that runs if none of the conditions turned out to be true.

**PostgreSQL**

```sql
IF condition THEN
    statements
[ELSIF condition THEN
    statements] ...
[ELSE
    statements]
END IF;
```

- `condition` — a logical expression, for example `student_age < 18`. The first branch whose condition
  is true runs, the rest are skipped.
- `statements` — one or more SQL statements, each ending with `;`. They run if this branch is chosen.
- `ELSIF` (keyword, optional) — another branch with its own condition. It's checked only if all the conditions above
  are false. You can add several of these branches.
- `ELSE` (keyword, optional) — the branch that runs if none of the conditions turned out to be true.

### IF Usage Example

**MySQL**

Let's create a procedure that categorizes students by age:

```sql
CREATE PROCEDURE categorize_student_by_age(
    IN student_id INT,
    OUT category VARCHAR(20)
)
BEGIN
    DECLARE student_age INT;

    -- Get student age
    SELECT TIMESTAMPDIFF(YEAR, birthday, CURDATE())
    INTO student_age
    FROM Student
    WHERE id = student_id;

    -- Determine category by age
    IF student_age < 18 THEN
        SET category = 'Minor';
    ELSEIF student_age BETWEEN 18 AND 25 THEN
        SET category = 'Young';
    ELSE
        SET category = 'Adult';
    END IF;
END;

-- Use the procedure
CALL categorize_student_by_age(1, @category);
SELECT @category AS age_category;
```

**PostgreSQL**

Let's create a function that categorizes students by age:

```sql
CREATE OR REPLACE FUNCTION categorize_student_by_age(student_id INT)
RETURNS VARCHAR(20)
LANGUAGE plpgsql
AS $$
DECLARE
    student_age INT;
    category VARCHAR(20);
BEGIN
    -- Get student age
    SELECT EXTRACT(YEAR FROM AGE(CURRENT_DATE, birthday))
    INTO student_age
    FROM Student
    WHERE id = student_id;

    -- Determine category by age
    IF student_age < 18 THEN
        category := 'Minor';
    ELSIF student_age BETWEEN 18 AND 25 THEN
        category := 'Young';
    ELSE
        category := 'Adult';
    END IF;

    RETURN category;
END;
$$;

-- Use the function
SELECT categorize_student_by_age(1) AS age_category;
```

**MySQL**

| age_category |
| ------------ |
| Young        |

## CASE Selection Statement

The `CASE` operator provides a more elegant way to handle multiple conditions.

### CASE Syntax

**MySQL**

```sql
CASE
    WHEN condition THEN result
    [WHEN ...]
    [ELSE default_result]
END
```

- `condition` — a logical expression checked in its own `WHEN` branch. There can be several branches:
  the first one whose condition is true wins.
- `result` — the value `CASE` takes if this branch's condition is true.
- `default_result` (optional) — the value returned when none of the conditions is met. Without `ELSE`, `NULL` is
  returned.

**PostgreSQL**

```sql
CASE
    WHEN condition THEN result
    [WHEN ...]
    [ELSE default_result]
END
```

- `condition` — a logical expression checked in its own `WHEN` branch. There can be several branches:
  the first one whose condition is true wins.
- `result` — the value `CASE` takes if this branch's condition is true.
- `default_result` (optional) — the value returned when none of the conditions is met. Without `ELSE`, `NULL` is
  returned.

### CASE Usage Example

**MySQL**

Let's create the same student categorization procedure, but using the CASE operator:

```sql
CREATE PROCEDURE categorize_student_with_case(
    IN student_id INT,
    OUT category VARCHAR(20)
)
BEGIN
    DECLARE student_age INT;

    -- Get student age
    SELECT TIMESTAMPDIFF(YEAR, birthday, CURDATE())
    INTO student_age
    FROM Student
    WHERE id = student_id;

    -- Determine category using CASE
    SET category = CASE
        WHEN student_age < 18 THEN 'Minor'
        WHEN student_age BETWEEN 18 AND 25 THEN 'Young'
        ELSE 'Adult'
    END;
END;

-- Use the procedure
CALL categorize_student_with_case(1, @category);
SELECT @category AS age_category;
```

**PostgreSQL**

Let's create the same student categorization function, but using the CASE operator:

```sql
CREATE OR REPLACE FUNCTION categorize_student_with_case(student_id INT)
RETURNS VARCHAR(20)
LANGUAGE plpgsql
AS $$
DECLARE
    student_age INT;
    category VARCHAR(20);
BEGIN
    -- Get student age
    SELECT EXTRACT(YEAR FROM AGE(CURRENT_DATE, birthday))
    INTO student_age
    FROM Student
    WHERE id = student_id;

    -- Determine category using CASE
    category := CASE
        WHEN student_age < 18 THEN 'Minor'
        WHEN student_age BETWEEN 18 AND 25 THEN 'Young'
        ELSE 'Adult'
    END;

    RETURN category;
END;
$$;

-- Use the function
SELECT categorize_student_with_case(1) AS age_category;
```

**MySQL**

| age_category |
| ------------ |
| Young        |

## WHILE Loop

The `WHILE` loop allows you to execute code repeatedly while a certain condition is met.

### WHILE Syntax

**MySQL**

```sql
WHILE condition DO
    statements
END WHILE;
```

- `condition` — a logical expression checked before each pass of the loop. The statements repeat while
  it's true.
- `statements` — the SQL statements in the loop body. One of them should eventually make the condition
  false, otherwise the loop never ends.

**PostgreSQL**

```sql
WHILE condition LOOP
    statements
END LOOP;
```

- `condition` — a logical expression checked before each pass of the loop. The statements repeat while
  it's true.
- `statements` — the SQL statements in the loop body. One of them should eventually make the condition
  false, otherwise the loop never ends.

### WHILE Usage Example

Let's look at an example of a stored procedure for creating several test subjects:

**MySQL**

```sql
CREATE PROCEDURE create_test_subjects(IN count_subjects INT)
BEGIN
    DECLARE i INT DEFAULT 1;
    DECLARE subject_id INT DEFAULT 20;

    WHILE i <= count_subjects DO
        INSERT INTO Subject (id, name)
        VALUES
        (
            subject_id + i,
            CONCAT('Test Subject ', i)
        );

        SET i = i + 1;
    END WHILE;
END;

-- Create 3 test subjects
CALL create_test_subjects(3);

-- Check the result
SELECT * FROM Subject WHERE name LIKE 'Test Subject%';
```

**PostgreSQL**

```sql
CREATE OR REPLACE PROCEDURE create_test_subjects(count_subjects INT)
LANGUAGE plpgsql
AS $$
DECLARE
    i INT := 1;
    subject_id INT := 20;
BEGIN
    WHILE i <= count_subjects LOOP
        INSERT INTO Subject (id, name)
        VALUES
        (
            subject_id + i,
            'Test Subject ' || i
        );

        i := i + 1;
    END LOOP;

    RAISE NOTICE 'Created % test subjects', count_subjects;
END;
$$;

-- Create 3 test subjects
CALL create_test_subjects(3);

-- Check the result
SELECT * FROM Subject WHERE name LIKE 'Test Subject%';
```

| id  | name           |
| --- | -------------- |
| 21  | Test Subject 1 |
| 22  | Test Subject 2 |
| 23  | Test Subject 3 |

Flow control operators make stored procedures and functions a powerful tool for implementing complex business logic directly in the database! 🚀
