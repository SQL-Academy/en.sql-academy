---
meta:
    title: 'LIKE operator'
    description: 'SQL syntax of the LIKE operator to search for records by a patterned string'
---

# LIKE operator

The `LIKE` operator is used in conditional queries when we want to find out whether a string matches a certain pattern.

For example, we have a `Users` table that has an `email` field:

```sql
SELECT name, email FROM Users;
```

| name              | email                 |
| ----------------- | --------------------- |
| Bruce Willis      | barjam@hotmail.com    |
| George Clooney    | tellis@me.com         |
| Kevin Costner     | metzzo@hotmail.com    |
| Samuel L. Jackson | moonlapse@outlook.com |
| Kurt Russell      | gator@live.com        |

Suppose we want to find all users whose email is in the second-level domain "hotmail".
That is, we need to select only those records that meet the following condition:

- after the "@" symbol, "hotmail" follows
- after "hotmail," a "." symbol follows and then any sequence of characters

For such non-trivial searches on string fields, we need the `LIKE` operator.

## Syntax

```sql
... WHERE table_field [NOT] LIKE string_pattern
```

The pattern may include the following special characters:

| Character | Description                                                                               |
| :-------- | :---------------------------------------------------------------------------------------- |
| `%`       | Any sequence of characters (the number of characters in the sequence can be zero or more) |
| `_`       | Any single character                                                                      |

So our query for finding users in the "hotmail" domain might look like this:

```sql
SELECT name, email FROM Users
WHERE email LIKE '%@hotmail.%'
```

| name                 | email                |
| -------------------- | -------------------- |
| Bruce Willis         | barjam@hotmail.com   |
| Kevin Costner        | metzzo@hotmail.com   |
| Jennifer Lopez       | barjam@hotmail.com   |
| Harrison Ford        | kostas@hotmail.com   |
| Michael Douglas      | timtroyr@hotmail.com |
| Catherine Zeta-Jones | flakeg@hotmail.com   |

## Examples

- ```sql
  ... WHERE table_field LIKE 'text%'
  ```

  Matches any strings beginning with "text"

- ```sql
  ... WHERE table_field LIKE '%text'
  ```

  Matches any strings ending with "text"

- ```sql
  ... WHERE table_field LIKE '_ext'
  ```

  Matches strings with a length of 4 characters, with the last 3 characters required to be "ext". For example, the words "text" and "next"

- ```sql
  ... WHERE table_field LIKE 'begin%end'
  ```
  Matches strings starting with "begin" and ending with "end"

> By default, MySQL is not case-sensitive.

## ESCAPE character

The ESCAPE character is used to escape special characters (`%` and `_`).
In case you need to find strings containing these special characters as literal text, you can use the ESCAPE character.

### Syntax with ESCAPE

```sql
... WHERE table_field LIKE 'string_pattern' ESCAPE 'escape_character'
```

For example, if you want to retrieve the job IDs of tasks with a progress of 3%:

```sql
SELECT job_id FROM Jobs
WHERE progress LIKE '3!%' ESCAPE '!';
```

If we didn't escape the wildcard character, the query would include everything starting with 3.
