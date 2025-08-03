---
meta:
  title: "String data type in SQL: MySQL and PostgreSQL"
  description: "Working with strings in MySQL and PostgreSQL. Basic data types for text information."
---

# String data type

The string data type is the most commonly used data type. Thanks to this, both text and various binary data (for example, pictures) are stored in the database.

<MySQLOnly>

In MySQL, it is represented by the following types:

## CHAR and VARCHAR

| Type         | Description                                                                                                                                                                                          | Range of characters                                 |
| :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------- |
| `CHAR(X)`    | Contains non-binary strings. The length is fixed, you specify it when declaring. If the length of the string is less than the specified one, it is padded with right spaces to the specified length. | The length can be any in the range from 0 to 255    |
| `VARCHAR(X)` | Contains non-binary strings. The length of the lines is dynamic.                                                                                                                                     | The length can be any in the range from 0 to 65,535 |

## BINARY and VARBINARY

The `BINARY` and `VARBINARY` data types are similar to `VARCHAR` and `CHAR` except that they store binary strings.

| Type           | Description                                                                  | Range of characters                                 |
| :------------- | :--------------------------------------------------------------------------- | :-------------------------------------------------- |
| `BINARY(X)`    | Contains binary strings. The length is fixed, you specify it when declaring. | The length can be any in the range from 0 to 255    |
| `VARBINARY(X)` | Contains binary strings. The length of the lines is dynamic.                 | The length can be any in the range from 0 to 65,535 |

## BLOB and TEXT

`BLOB` is used to store large binary data such as pictures. `TEXT` is also for storing big data, but textual content.

The difference between them is that the sorts and comparisons of stored data for `BLOB` are case sensitive and not case sensitive in `TEXT` fields.

| Type   | Description              | Range of characters   |
| :----- | :----------------------- | :-------------------- |
| `BLOB` | Contains binary strings. | Maximum length 65,535 |
| `TEXT` | Contains text lines.     | Maximum length 65,535 |

`BLOB` and `TEXT` have additional subtypes that differ in the maximum data size that can be stored in them.

| Type         | Range of characters          |
| :----------- | :--------------------------- |
| `TINYBLOB`   | Maximum length 255           |
| `MEDIUMBLOB` | Maximum length 16,777,215    |
| `LONGBLOB`   | Maximum length 4,294,967,295 |
| `TINYTEXT`   | Maximum length 255           |
| `MEDIUMTEXT` | Maximum length 16,777,215    |
| `LONGTEXT`   | Maximum length 4,294,967,295 |

</MySQLOnly>

<PostgreSQLOnly>

In PostgreSQL, it is represented by the following types:

## CHARACTER and VARCHAR

| Type         | Description                                                                    | Range of characters                                 |
| :----------- | :----------------------------------------------------------------------------- | :-------------------------------------------------- |
| `CHAR(n)`    | Contains text strings of fixed length. Padded with spaces to specified length. | Length can be any in the range from 1 to 10,485,760 |
| `VARCHAR(n)` | Contains text strings of variable length with limitation.                      | Length can be any in the range from 1 to 10,485,760 |
| `TEXT`       | Contains text strings of unlimited variable length.                            | Practically unlimited length (up to 1 GB)           |

It's important to note that in PostgreSQL, the `TEXT` type is preferable to use instead of `VARCHAR` without length limitation, as they have the same performance.

</PostgreSQLOnly>
