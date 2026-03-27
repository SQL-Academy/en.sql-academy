---
meta:
  title: "LIKE operator"
  description: "SQL syntax of the LIKE and ILIKE operators to search for records by a patterned string"
---

# LIKE operator

The `LIKE` operator is used in conditional queries when we want to find out whether a string matches a certain pattern.

For example, we have a `Users` table that has an `email` field:

```sql-executable-Airbnb
SELECT name, email FROM Users;
```

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

```sql-executable-Airbnb
SELECT name, email FROM Users
WHERE email LIKE '%@hotmail.%'
```

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

<MySQLOnly>

> By default, MySQL patterns are not case-sensitive

</MySQLOnly>

<PostgreSQLOnly>

> In PostgreSQL, patterns are case-sensitive. For case-insensitive search, use the `ILIKE` operator

</PostgreSQLOnly>

## Escaping special characters

Sometimes you need to search for strings where `%` and `_` are part of the text itself rather than pattern elements. In such cases, these characters must be escaped.
In `LIKE` patterns, `\` is used as the default escape character. For example, if you need to retrieve the IDs of tasks with a progress of `3%`, you can write:

```sql
SELECT job_id FROM Jobs
WHERE progress LIKE '3\\%';
```

### ESCAPE character

If you want to use a different escape character instead of the default one, you can specify it explicitly with `ESCAPE`.

### Syntax with ESCAPE

```sql
... WHERE table_field LIKE 'string_pattern' ESCAPE 'escape_character'
```

The same query can be written with an explicitly specified escape character:

```sql
SELECT job_id FROM Jobs
WHERE progress LIKE '3!%' ESCAPE '!';
```

Here, `!` plays the same role as `\` in the previous example.

## Interactive Exercise

Now let's practice what we've learned!  
In the exercise below, you need to distribute email addresses into columns according to the given `LIKE` patterns.
