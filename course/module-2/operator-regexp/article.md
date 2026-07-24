---
meta:
    title: "REGEXP and ~ Operators in SQL"
    description: "Using regular expressions in SQL with REGEXP operator in MySQL and ~ operator in PostgreSQL"
---

**MySQL**

# REGEXP Operator for Regular Expressions

The `REGEXP` operator (or its synonym `RLIKE`) in MySQL is used for searching and manipulating string data
using regular expressions.

**PostgreSQL**

# Regular Expression Operator \~

The `~` and `~*` operators in PostgreSQL are used for searching and manipulating string data
using regular expressions.

Regular expressions provide powerful capabilities for complex search patterns that are difficult
to implement with the `LIKE` operator.

## When to use regular expressions instead of LIKE?

The `LIKE` operator is convenient for simple search patterns,
such as finding strings that start or end with a certain set of characters or contain certain substrings.
However, if a more complex and flexible search is required, such as searching by multiple conditions
or using special characters and ranges, regular expression operators become an indispensable tool.

It is important to remember that `LIKE` compares the whole string against its pattern, while a regular expression looks for a match inside the string. If you need to check the beginning or the end of the string specifically, use the special characters `^` and `$`.

## Regular expression syntax

**MySQL**

```sql
... WHERE table_field REGEXP 'pattern';
```

Where `pattern` is the regular expression that defines the search pattern.

**PostgreSQL**

```sql
... WHERE table_field ~ 'pattern';   -- case-sensitive
... WHERE table_field ~* 'pattern';  -- case-insensitive
```

Where `pattern` is the regular expression that defines the search pattern.

## Important Considerations

**MySQL**

1. **Case insensitive**

    By default, regular expressions in MySQL are not case-sensitive.\
    For example, the expression `REGEXP 'abc'` will match the string `abc`, `Abc`, and `ABC`.

2. **Special characters**

    Some characters have special meanings in regular expressions and require escaping (e.g.,
    `.`, `*`, `+`, `?`, `[`, `]`, `(`, `)`, `{`, `}`, `|`, `\`).

    To escape such characters, use a double backslash — `\\`.

**PostgreSQL**

1. **Case sensitivity**

    By default, regular expressions in PostgreSQL are case-sensitive.

    - The `~` operator — case-sensitive
    - The `~*` operator — case-insensitive

2. **Special characters**

    Some characters have special meanings in regular expressions and require escaping (e.g.,
    `.`, `*`, `+`, `?`, `[`, `]`, `(`, `)`, `{`, `}`, `|`, `\`).

    To escape such characters, use a single backslash — `\`.

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

    **MySQL**

    ```sql
    SELECT * FROM Users WHERE name REGEXP '^John'
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM Users WHERE name ~ '^John'
    ```

    | id  | name          | email             | email_verified_at        | password             | phone_number    |
    | --- | ------------- | ----------------- | ------------------------ | -------------------- | --------------- |
    | 18  | John Travolta | wainwrig\@msn.com | 2016-11-19T12:30:43.000Z | fzjhl0v82o0amalr8649 | +1 202 555 0176 |
    | 28  | Johnny Depp   | cgarcia\@yahoo.ca | 2017-05-26T01:19:06.000Z | qpp6hbnae42cdhmxlk4j | +7 401 195 7363 |

    This expression searches for strings starting with "John". The `^` symbol indicates the start of the string.

- **Display all school subjects whose names end with the letter "e" or "y":**

    **MySQL**

    ```sql
    SELECT * FROM  Subject WHERE name REGEXP '[ey]$'
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM  Subject WHERE name ~ '[ey]$'
    ```

    | id  | name             |
    | --- | ---------------- |
    | 2   | Russian language |
    | 3   | Literature       |
    | 5   | Chemistry        |
    | 6   | Geography        |
    | 7   | History          |
    | 8   | Biology          |
    | 9   | English language |
    | 11  | Physical Culture |
    | 13  | Technology       |

    In this example, `[ey]` defines a list of possible values for the pattern `$`, which defines what the string should end with.

- **Find all users whose email addresses end with "@outlook.com" or "@icloud.com":**

    **MySQL**

    ```sql
    SELECT * FROM Users WHERE email REGEXP '@(outlook\\.com|icloud\\.com)$'
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM Users WHERE email ~ '@(outlook\.com|icloud\.com)$'
    ```

    | id  | name              | email                  | email_verified_at        | password             | phone_number      |
    | --- | ----------------- | ---------------------- | ------------------------ | -------------------- | ----------------- |
    | 7   | Samuel L. Jackson | moonlapse\@outlook.com | 2018-07-19T11:16:13.000Z | i6yvht95527z3idgqx9y | +1 202 555 0162   |
    | 13  | Steve Martin      | nelson\@outlook.com    | 2016-07-29T04:25:00.000Z | w76yphg3kvzg77ilmxfs | +1 202 555 0138   |
    | 29  | Pierce Brosnan    | treeves\@icloud.com    | 2019-03-08T01:56:00.000Z | lqiwecclne9rv8woo2go | +7 401 749 3620   |
    | 30  | Sean Connery      | jschauma\@icloud.com   | 2016-05-21T00:45:17.000Z | lyh4jkdxkvtvulvqi5db | +7 401 511 6783   |
    | 31  | Bruce Willis      | kewley\@icloud.com     | 2016-12-08T20:18:59.000Z | 0ofa2khvnptiackbssv0 | +375 154 771 3462 |

    Here, `$` is used to indicate the end of the string and `|` is used to specify multiple options.

- **Find all users whose phone numbers do not contain the digits "2" and "8":**

    **MySQL**

    ```sql
    SELECT * FROM Users WHERE phone_number REGEXP '^[^28]*$'
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM Users WHERE phone_number ~ '^[^28]*$'
    ```

    | id  | name        | email                 | email_verified_at        | password             | phone_number    |
    | --- | ----------- | --------------------- | ------------------------ | -------------------- | --------------- |
    | 27  | Brad Pitt   | kewley\@optonline.net | 2017-02-11T05:45:15.000Z | 829j2ygocn8btzae49kv | +7 401 741 3797 |
    | 28  | Johnny Depp | cgarcia\@yahoo.ca     | 2017-05-26T01:19:06.000Z | qpp6hbnae42cdhmxlk4j | +7 401 195 7363 |

    In this example, the symbol `[^28]` represents any character except "2" and "8", and `*` means any number of such characters.
    The `^` and `$` symbols indicate the start and end of the string respectively, ensuring that the entire string matches the pattern.

- **Find all users whose phone number starts with «+7»**

    **MySQL**

    ```sql
    SELECT name, phone_number FROM Users WHERE phone_number REGEXP '^\\+7'
    ```

    **PostgreSQL**

    ```sql
    SELECT name, phone_number FROM Users WHERE phone_number ~ '^\+7'
    ```

    | name           | phone_number    |
    | -------------- | --------------- |
    | Hideo Kojima   | +7 401 452 0052 |
    | ClINT Eastwood | +7 401 722 0912 |
    | Brad Pitt      | +7 401 741 3797 |
    | Johnny Depp    | +7 401 195 7363 |
    | Pierce Brosnan | +7 401 749 3620 |
    | Sean Connery   | +7 401 511 6783 |

    In this example, `^` denotes the beginning of a string. This means we are looking for strings that start with a specific pattern.

    **MySQL**

    Since `+` is a special character in regular expressions,
    it needs to be escaped with a double backslash (`\\`) so that it is treated as the literal `+` character.
    As a result, `\\+` matches the `+` sign in the string.

    **PostgreSQL**

    Since `+` is a special character in regular expressions,
    it needs to be escaped with a single backslash (`\`) so that it is treated as the literal `+` character.
    As a result, `\+` matches the `+` sign in the string.
