---
meta:
    title: 'Using functions'
    description: 'Examples of scalar SQL functions and their applications on literals and field values from tables.'
---

# Using Functions

When creating SQL queries, we can use built-in functions. For example, if we want to output a string in uppercase, we can use the `UPPER` function.

```sql
SELECT UPPER("Hello world") AS upper_string;
```

| upper_string |
| ------------ |
| HELLO WORLD  |

## What is a built-in function?

The built–in function is a piece of code implemented in the DBMS, with which you can perform transformations of string, numeric and other data in queries.

Each function accepts a set of arguments of a certain type, performs the operations inherent in it and necessarily returns one of the possible literals. It is worth noting that functions can take either zero arguments or several.

For example, the function `NOW()` takes zero arguments and returns a literal in date format, and `LENGTH('sql-academy')` takes one string argument and returns the numeric literal "11".

## Examples of Functions

There are many functions, but the main ones can always be found using the search bar in the header or on the <a href="/handbook/substring" target="_blank">function reference page</a>.

Here are some examples:

-   <a href="/handbook/lower" target="_blank">
      **LOWER**
    </a>

    Returns a string in which all characters are written in lowercase

    ```sql
    SELECT LOWER('SQL Academy') AS lower_string;
    ```

    | lower_string |
    | ------------ |
    | sql academy  |

-   <a href="/handbook/year" target="_blank">
      **YEAR**
    </a>

    Returns the year for a given date.

    ```sql
    SELECT YEAR("2022-06-16") AS year;
    ```

    | year |
    | ---- |
    | 2022 |

-   <a href="/handbook/instr" target="_blank">
      **INSTR**
    </a>

    Searches for a substring in a string, returning the position of its first character. At the same time, the countdown
    it starts with one, not zero, as in most programming languages.

    The function works by character-by-character comparison of the source string with the desired one. For example, in the string `sql-academy`, the substring `academy` appears starting from the fifth character.

    ```sql
    SELECT INSTR('sql-academy', 'academy') AS idx;
    ```

    | idx |
    | --- |
    | 5   |

-   <a href="/handbook/length" target="_blank">
      **LENGTH**
    </a>

    Returns the length of the specified string.

    ```sql
    SELECT LENGTH('sql-academy') AS str_length;
    ```

    | str_length |
    | ---------- |
    | 11         |

## Applying functions over table field values

Functions can be used not only on literals, but also on values taken from a table. In this case, the function performs transformations for each row separately.

For example, let's go back to our database and look at the `FamilyMembers` table:
it contains the name, status, and birthdate of people.

We can modify each of these fields' values when outputting them. The following query calculates the length of the full name for each family member.

```sql-format
SELECT member_name, LENGTH(member_name) AS fullname_length FROM FamilyMembers;
```

| member_name       | fullname_length |
| ----------------- | --------------- |
| Headley Quincey   | 15              |
| Flavia Quincey    | 14              |
| Andie Quincey     | 13              |
| Lela Quincey      | 12              |
| Annie Quincey     | 13              |
| Ernest Forrest    | 14              |
| Constance Forrest | 17              |
| Wednesday Addams  | 16              |

## Operations on the result of the function

Since we know that each function must return any of the possible literals, its result can also be used in further calculations and transformations.

For example, we want to get the first three letters in a string and convert them to uppercase. To do this, it will be enough for us to combine two functions: `LEFT` and `UPPER`, where the result of one function will be an argument for the second.

```sql-format
SELECT UPPER(LEFT('sql-academy', 3)) AS str;
```

| str |
| --- |
| SQL |

Or we want to calculate the length of a person's last name by having a string in the format `first name<space>last name`. One of the possible ways to calculate the length of the last name can be using the functions `LENGTH` and `INSTR`, using the formula `<length of the last name> = <length of the entire string> - (<length of the name> + <length of the space>)`:

-   The value `<length of the entire string>` can be obtained using the `LENGTH` function
-   For `<length of the name> + <length of the space>`, you need to calculate the position of the character where the name ends and add one, because the space has a length of "1". We can do this using only the `INSTR` function, focusing on the "space" character

Since both functions return numeric literals, we can perform arithmetic operations on them. Let's subtract one from the other and get the length of the last name (lastname_length):

```sql-format
SELECT
    member_name,
    LENGTH(member_name) AS full_length,
    INSTR(member_name, ' ') AS firstname_with_space_length,
    LENGTH(member_name) - INSTR(member_name, ' ') AS lastname_length
FROM FamilyMembers;
```

| member_name       | full_length | firstname_with_space_length | lastname_length |
| ----------------- | ----------- | --------------------------- | --------------- |
| Headley Quincey   | 15          | 8                           | 7               |
| Flavia Quincey    | 14          | 7                           | 7               |
| Andie Quincey     | 13          | 6                           | 7               |
| Lela Quincey      | 12          | 5                           | 7               |
| Annie Quincey     | 13          | 6                           | 7               |
| Ernest Forrest    | 14          | 7                           | 7               |
| Constance Forrest | 17          | 10                          | 7               |
| Wednesday Addams  | 16          | 10                          | 6               |
