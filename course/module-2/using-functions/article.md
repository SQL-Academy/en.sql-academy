---
meta:
    title: 'Using functions'
    description: 'Examples of scalar SQL functions and their applications on literals and field values from tables.'
---

# Using Functions

When creating SQL queries, we can use built-in functions. For example, if we want to output a string in uppercase, we can use the `UPPER` function.

```sql-executable
SELECT UPPER('Hello world') AS upper_string;
```

## What is a built-in function?

The built–in function is a piece of code implemented in the DBMS, with which you can perform transformations of string, numeric and other data in queries.

Each function accepts a set of arguments of a certain type, performs the operations inherent in it and necessarily returns one of the possible literals. It is worth noting that functions can take either zero arguments or several.

For example, the function `NOW()` takes zero arguments and returns a literal in date format, and `LENGTH('sql-academy')` takes one string argument and returns the numeric literal "11".

## Examples of Functions

There are many functions, but the main ones can always be found using the search bar in the header or on the <a href="/handbook" target="_blank">function reference page</a>.

Here are some examples:

<MySQLOnly>

-   <a href="/handbook/mysql/lower" target="_blank">
        **LOWER**
    </a>

    Returns a string in which all characters are written in lowercase

    ```sql-executable
    SELECT LOWER('SQL Academy') AS lower_string;
    ```

-   <a href="/handbook/mysql/year" target="_blank">
        **YEAR**
    </a>

    Returns the year for a given date.

    ```sql-executable
    SELECT YEAR('2022-06-16') AS year;
    ```

-   <a href="/handbook/mysql/instr" target="_blank">
        **INSTR**
    </a>

    Searches for a substring in a string, returning the position of its first character. At the same time, the countdown
    starts with one, not zero, as in most programming languages.

    The function works by character-by-character comparison of the source string with the desired one. For example, in the string `sql-academy`, the substring `academy` appears starting from the fifth character.

    ```sql-executable
    SELECT INSTR('sql-academy', 'academy') AS idx;
    ```

-   <a href="/handbook/mysql/length" target="_blank">
        **LENGTH**
    </a>

    Returns the length of the specified string.

    ```sql-executable
    SELECT LENGTH('sql-academy') AS str_length;
    ```

</MySQLOnly>

<PostgreSQLOnly>

-   <a href="/handbook/postgresql/lower" target="_blank">
        **LOWER**
    </a>

    Returns a string in which all characters are written in lowercase

    ```sql-executable
    SELECT LOWER('SQL Academy') AS lower_string;
    ```

-   <a href="/handbook/postgresql/extract" target="_blank">
        **EXTRACT**
    </a>

    Extracts date parts (year, month, day, etc.) from a given date

    ```sql-executable
    SELECT EXTRACT(YEAR FROM DATE '2022-06-16') AS year;
    ```

-   <a href="/handbook/postgresql/position" target="_blank">
        **POSITION**
    </a>

    Searches for a substring in a string, returning the position of its first character. At the same time, the countdown
    starts with one, not zero, as in most programming languages.

    The function works by character-by-character comparison of the source string with the desired one. For example, in the string `sql-academy`, the substring `academy` appears starting from the fifth character.

    ```sql-executable
    SELECT POSITION('academy' IN 'sql-academy') AS idx;
    ```

-   <a href="/handbook/postgresql/length" target="_blank">
        **LENGTH**
    </a>

    Returns the length of the specified string.

    ```sql-executable
    SELECT LENGTH('sql-academy') AS str_length;
    ```

</PostgreSQLOnly>

## Applying functions over table field values

Functions can be used not only on literals, but also on values taken from a table. In this case, the function performs transformations for each row separately.

For example, let's go back to our database and look at the `FamilyMembers` table:
it contains the name, status, and birthdate of people.

We can modify each of these fields' values when outputting them. The following query calculates the length of the full name for each family member.

```sql-executable-Family-format
SELECT member_name, LENGTH(member_name) AS fullname_length FROM FamilyMembers;
```

## Operations on the result of the function

Since we know that each function must return any of the possible literals, its result can also be used in further calculations and transformations.

For example, we want to get the first three letters in a string and convert them to uppercase. To do this, it will be enough for us to combine two functions: `LEFT` and `UPPER`, where the result of one function will be an argument for the second.

```sql-executable-format
SELECT UPPER(LEFT('sql-academy', 3)) AS str;
```

Or we want to calculate the length of a person's last name by having a string in the format `first name<space>last name`. One of the possible ways to calculate the length of the last name can be using the functions `LENGTH` and position search, using the formula `<length of the last name> = <length of the entire string> - (<length of the name> + <length of the space>)`:

-   The value `<length of the entire string>` can be obtained using the `LENGTH` function

<MySQLOnly>

-   For `<length of the name> + <length of the space>`, you need to calculate the position of the character where the name ends and add one, because the space has a length of "1". We can do this using only the `INSTR` function, focusing on the "space" character

Since both functions return numeric literals, we can perform arithmetic operations on them. Let's subtract one from the other and get the length of the last name (lastname_length):

```sql-executable-Family-format
SELECT
    member_name,
    LENGTH(member_name) AS full_length,
    INSTR(member_name, ' ') AS firstname_with_space_length,
    LENGTH(member_name) - INSTR(member_name, ' ') AS lastname_length
FROM FamilyMembers;
```

</MySQLOnly>

<PostgreSQLOnly>

-   For `<length of the name> + <length of the space>`, you need to calculate the position of the character where the name ends and add one, because the space has a length of "1". We can do this using only the `POSITION` function, focusing on the "space" character

Since both functions return numeric literals, we can perform arithmetic operations on them. Let's subtract one from the other and get the length of the last name (lastname_length):

```sql-executable-Family-format
SELECT
    member_name,
    LENGTH(member_name) AS full_length,
    POSITION(' ' IN member_name) AS firstname_with_space_length,
    LENGTH(member_name) - POSITION(' ' IN member_name) AS lastname_length
FROM FamilyMembers;
```

</PostgreSQLOnly>
