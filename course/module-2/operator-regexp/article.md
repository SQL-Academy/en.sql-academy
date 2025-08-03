---
meta:
  title: "REGEXP and ~ Operators in SQL"
  description: "Using regular expressions in SQL with REGEXP operator in MySQL and ~ operator in PostgreSQL"
---

<MySQLOnly>

# REGEXP Operator for Regular Expressions

The `REGEXP` operator (or its synonym `RLIKE`) in MySQL is used for searching and manipulating string data
using regular expressions.

</MySQLOnly>

<PostgreSQLOnly>

# Regular Expression Operator ~

The `~` and `~*` operators in PostgreSQL are used for searching and manipulating string data
using regular expressions.

</PostgreSQLOnly>

Regular expressions provide powerful capabilities for complex search patterns that are difficult
to implement with the `LIKE` operator.

## When to use regular expressions instead of LIKE?

The `LIKE` operator is convenient for simple search patterns,
such as finding strings that start or end with a certain set of characters or contain certain substrings.
However, if a more complex and flexible search is required, such as searching by multiple conditions
or using special characters and ranges, regular expression operators become an indispensable tool.

## Regular expression syntax

<MySQLOnly>

```sql
... WHERE table_field REGEXP 'pattern';
```

Where `pattern` is the regular expression that defines the search pattern.

</MySQLOnly>

<PostgreSQLOnly>

```sql
... WHERE table_field ~ 'pattern';   -- case-sensitive
... WHERE table_field ~* 'pattern';  -- case-insensitive
```

Where `pattern` is the regular expression that defines the search pattern.

</PostgreSQLOnly>

## Important Considerations

<MySQLOnly>

1. **Case insensitive**

   By default, regular expressions in MySQL are not case-sensitive.  
   For example, the expression `REGEXP 'abc'` will match the string `abc`, `Abc`, and `ABC`.

2. **Special characters**

   Some characters have special meanings in regular expressions and require escaping (e.g.,
   `.`, `*`, `+`, `?`, `[`, `]`, `(`, `)`, `{`, `}`, `|`, `\`).

   To escape such characters, use a double backslash — `\\`.

</MySQLOnly>

<PostgreSQLOnly>

1. **Case sensitivity**

   By default, regular expressions in PostgreSQL are case-sensitive.

   - The `~` operator — case-sensitive
   - The `~*` operator — case-insensitive

2. **Special characters**

   Some characters have special meanings in regular expressions and require escaping (e.g.,
   `.`, `*`, `+`, `?`, `[`, `]`, `(`, `)`, `{`, `}`, `|`, `\`).

   To escape such characters, use a single backslash — `\`.

</PostgreSQLOnly>

## Special characters and constructs

| Characters and constructs | Matches                                               |
| :------------------------ | :---------------------------------------------------- |
| `*`                       | 0 or more instances of the preceding string           |
| `+`                       | 1 or more instances of the preceding string           |
| `.`                       | Any single character                                  |
| `?`                       | 0 or 1 instance of the preceding string               |
| `^`                       | Matches the start of the string                       |
| `$`                       | Matches the end of the string                         |
| `[abc]`                   | Any character listed in the square brackets           |
| `[^abc]`                  | Any character not listed in the square brackets       |
| `[A-Z]`                   | Matches any uppercase letter                          |
| `[a-z]`                   | Matches any lowercase letter                          |
| `[0-9]`                   | Matches any digit                                     |
| `p1\|p2\|p3`              | Matches any of the patterns `p1` or `p2` or `p3`      |
| `{n}`                     | `n` instances of the preceding string                 |
| `{m,n}`                   | Between `m` and `n` instances of the preceding string |

## Examples with explanation

- **Get all users whose names start with "John":**

  <MySQLOnly>

  ```sql-executable-Airbnb
  SELECT * FROM Users WHERE name REGEXP '^John'
  ```

  </MySQLOnly>

  <PostgreSQLOnly>

  ```sql-executable-Airbnb
  SELECT * FROM Users WHERE name ~ '^John'
  ```

  </PostgreSQLOnly>

  This expression searches for strings starting with "John". The `^` symbol indicates the start of the string.

- **Display all school subjects whose names end with the letter "e" or "y":**

  <MySQLOnly>

  ```sql-executable-Schedule
  SELECT * FROM  Subject WHERE name REGEXP '[ey]$'
  ```

  </MySQLOnly>

  <PostgreSQLOnly>

  ```sql-executable-Schedule
  SELECT * FROM  Subject WHERE name ~ '[ey]$'
  ```

  </PostgreSQLOnly>

  In this example, `[ey]` defines a list of possible values for the pattern `$`, which defines what the string should end with.

- **Find all users whose email addresses end with "@outlook.com" or "@icloud.com":**

  <MySQLOnly>

  ```sql-executable-Airbnb
  SELECT * FROM Users WHERE email REGEXP '@(outlook.com|icloud.com)$'
  ```

  </MySQLOnly>

  <PostgreSQLOnly>

  ```sql-executable-Airbnb
  SELECT * FROM Users WHERE email ~ '@(outlook\.com|icloud\.com)$'
  ```

  </PostgreSQLOnly>

  Here, `$` is used to indicate the end of the string and `|` is used to specify multiple options.

- **Find all users whose phone numbers do not contain the digits "2" and "8":**

  <MySQLOnly>

  ```sql-executable-Airbnb
  SELECT * FROM Users WHERE phone_number REGEXP '^[^28]*$'
  ```

  </MySQLOnly>

  <PostgreSQLOnly>

  ```sql-executable-Airbnb
  SELECT * FROM Users WHERE phone_number ~ '^[^28]*$'
  ```

  </PostgreSQLOnly>

  In this example, the symbol `[^28]` represents any character except "2" and "8", and `*` means any number of such characters.
  The `^` and `$` symbols indicate the start and end of the string respectively, ensuring that the entire string matches the pattern.

- **Find all users whose phone number starts with «+7»**

  <MySQLOnly>

  ```sql-executable-Airbnb
  SELECT name, phone_number FROM Users WHERE phone_number REGEXP '^\\+7'
  ```

  </MySQLOnly>

  <PostgreSQLOnly>

  ```sql-executable-Airbnb
  SELECT name, phone_number FROM Users WHERE phone_number ~ '^\+7'
  ```

  </PostgreSQLOnly>

  In this example, `^` denotes the beginning of a string. This means we are looking for strings that start with a specific pattern.

  <MySQLOnly>

  Since `+` is a special character in regular expressions,
  it needs to be escaped with a double backslash (`\\`) so that it is treated as the literal `+` character.
  As a result, `\\+` matches the `+` sign in the string.

  </MySQLOnly>

  <PostgreSQLOnly>

  Since `+` is a special character in regular expressions,
  it needs to be escaped with a single backslash (`\`) so that it is treated as the literal `+` character.
  As a result, `\+` matches the `+` sign in the string.

  </PostgreSQLOnly>
