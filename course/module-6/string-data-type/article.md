---
meta:
    title: 'String data type in SQL'
    description: 'Working with strings in MySQL. Basic functions for working with text data.'
---

# String data type

The string data type is the most commonly used data type. Thanks to this, both text and various binary data (for example, pictures) are stored in the database.

In MySQL, it is represented by the following types:

## CHAR and VARCHAR

| Type         | Description                                                                                                                                                                                          | Range of characters                                 |
| :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------- |
| `CHAR(X)`    | Contains non-binary strings. The length is fixed, you specify it when declaring. If the length of the string is less than the specified one, it is padded with right spaces to the specified length. | The length can be any in the range from 0 to 255    |
| `VARCHAR(X)` | Contains non-binary strings. The length of the lines is dynamic.                                                                                                                                     | The length can be any in the range from 0 to 65.535 |

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
| `BLOB` | Contains binary strings. | Maximum length 65.535 |
| `TEXT` | Contains text lines.     | Maximum length 65.535 |

`BLOB` and `TEXT` have additional subtypes that differ in the maximum data size that can be stored in them.

| Type         | Range of characters          |
| :----------- | :--------------------------- |
| `TINYBLOB`   | Maximum length 255           |
| `MEDIUMBLOB` | Maximum length 16,777,215    |
| `LONGBLOB`   | Maximum length 4,294,967,295 |
| `TINYTEXT`   | Maximum length 255           |
| `MEDIUMTEXT` | Maximum length 16,777,215    |
| `LONGTEXT`   | Maximum length 4,294,967,295 |
