---
meta:
    title: 'IF, CASE, WHILE Operators in Stored Procedures'
    description: 'Learn conditional statements and loops in SQL stored procedures. Syntax and examples of IF, CASE, WHILE for MySQL and PostgreSQL.'
---

# IF, CASE, WHILE Operators in Stored Procedures

Stored procedures and functions are not just convenient containers for groups of queries. They allow you to implement quite complex logic using conditional operators and loops.

In this article, we'll explore the main flow control operators: conditional statements `IF` and `CASE`, as well as `WHILE` loops.

## IF Conditional Statement

The `IF` operator allows you to execute code based on whether a condition is met.

### IF Syntax

<MySQLOnly>

```sql
IF condition THEN
    -- code executed when condition is true
ELSEIF another_condition THEN
    -- code for alternative condition
ELSE
    -- default code
END IF;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
IF condition THEN
    -- code executed when condition is true
ELSIF another_condition THEN
    -- code for alternative condition
ELSE
    -- default code
END IF;
```

</PostgreSQLOnly>

### IF Usage Example

Let's create a procedure that categorizes students by age:

<MySQLOnly>

```sql-executable-Schedule
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

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Schedule
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

</PostgreSQLOnly>

## CASE Selection Statement

The `CASE` operator provides a more elegant way to handle multiple conditions.

### CASE Syntax

<MySQLOnly>

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END CASE;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END CASE;
```

</PostgreSQLOnly>

### CASE Usage Example

Let's create the same student categorization procedure, but using the CASE operator:

<MySQLOnly>

```sql-executable-Schedule
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

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Schedule
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

</PostgreSQLOnly>

## WHILE Loop

The `WHILE` loop allows you to execute code repeatedly while a certain condition is met.

### WHILE Syntax

<MySQLOnly>

```sql
WHILE condition DO
    -- code executed in the loop
END WHILE;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
WHILE condition LOOP
    -- code executed in the loop
END LOOP;
```

</PostgreSQLOnly>

### WHILE Usage Example

Let's create a procedure to create several test subjects:

<MySQLOnly>

```sql-executable-Schedule
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

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Schedule
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

</PostgreSQLOnly>


Flow control operators make stored procedures and functions a powerful tool for implementing complex business logic directly in the database! 🚀
