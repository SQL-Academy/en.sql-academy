# Conditional logic, CASE statement

SQL, like many programming languages, allows you to write conditional logic so that behavior
query depended on the values ​​of certain columns or expressions. In this article, we will look at how this is implemented in SQL with
using the `CASE` operator.

## Concept of conditional logic

The concept of conditional logic means that the program has several execution paths depending on some condition.

The Schedule database has a `Student` table with a `birthday` field that reflects the student's date of birth. Let's say
in the selection, it is necessary to display not the date of birth itself, but the text value "Adult" or "Minor", depending on whether
if the student is 18 or not. This is an example of conditional logic, in which either one value or another should be displayed
depending on the specific condition.

The implementation of such a query using `CASE` might look like this:

```sql
SELECT first_name, last_name,
CASE
  WHEN TIMESTAMPDIFF(YEAR, birthday, NOW()) >= 18 THEN "Adult"
  ELSE "Minor"
END AS status
FROM Student
```

| first_name | last_name    | status |
| ---------- | ------------ | ------ |
| Nikolaj    | Sokolov      | Adult  |
| Vyacheslav | Eliseev      | Adult  |
| Ivan       | Efremov      | Adult  |
| Anatolij   | ZHdanov      | Minor  |
| Georgij    | Noskov       | Adult  |
| Artyom     | Sergeev      | Minor  |
| Arina      | Evseeva      | Adult  |
| Angelina   | Voroncova    | Adult  |
| Ekaterina  | Ustinova     | Adult  |
| Raisa      | Lapina       | Adult  |
| Leonid     | Ignatov      | Minor  |
| Snezhana   | Seliverstova | Adult  |
| Semyon     | Biryukov     | Adult  |
| Georgij    | Baranov      | Adult  |
| YUliya     | Vishnyakova  | Adult  |
| Valentina  | Bolshakova   | Adult  |
| Leonid     | Kryukov      | Adult  |
| Vladislav  | Cvetkov      | Adult  |
| Snezhana   | Morozova     | Adult  |
| Lyubov     | Borisova     | Adult  |
| Anfisa     | Kalashnikova | Adult  |
| Anna       | Osipova      | Adult  |

## CASE search expression syntax

```sql
CASE
    WHEN condition_1 THEN return_value_1
    WHEN condition_2 THEN return_value_2
    WHEN condition_n THEN return_value_n
    [ELSE default-return_value]
END
```

If `condition_1` returns true, then the `CASE` expression will return `return_value_1`, otherwise a check will be made
to `condition_2` and so on. If none of the proposed conditions is met, then `NULL` or `default_return_value` will be returned,
if the `ELSE` construct was used.

### Example

Let's take the `CASE` operator to implement the stages of schooling.

![School education stages](https://sql-academy.org/static/guidePage/case-expression/en_school_education_stages.png "School education stages")

```sql
SELECT name,
CASE
  WHEN SUBSTRING(name, 1, INSTR(name, ' ')) IN (10,11) THEN "High School"
  WHEN SUBSTRING(name, 1, INSTR(name, ' ')) IN (5,6,7,8,9) THEN "Middle school"
  ELSE "Elementary school"
END AS stage
FROM Class
```

| name | stage             |
| ---- | ----------------- |
| 8 A  | Middle school     |
| 8 B  | Middle school     |
| 9 C  | Middle school     |
| 9 B  | Middle school     |
| 9 A  | Middle school     |
| 10 B | High School       |
| 10 A | High School       |
| 11 B | High School       |
| 11 A | High School       |
| 7 A  | Middle school     |
| 7 B  | Middle school     |
| 6 A  | Middle school     |
| 6 B  | Middle school     |
| 5 A  | Middle school     |
| 5 B  | Middle school     |
| 4 A  | Elementary school |

- First we extract the class number from its name
  ```sql
  SUBSTRING(name, 1, INSTR(name, ' '))
  ```
- Next, we check for occurrences of this class number in the list of classes related to "High School" and "Middle school".
- If the class number doesn't match 5-11, we output "Elementary school".

## Syntax of a simple CASE expression

The `CASE` operator also has a simpler syntax, which is similar to the `CASE` search expression, but
is a little less flexible. Here is its syntax:

```sql
CASE value
    WHEN compare_value_1 THEN return_value_1
    WHEN compare_value_2 THEN return_value_2
    WHEN compare_value_n THEN return_value_n
    [ELSE default_return_value]
END
```

In this syntax, the `value` in `CASE` is compared in turn with the passed values ​​in `WHEN` and, if a match is found,
value, the value following `THEN` is returned.

Using this syntax, we can rewrite our previous example:

```sql
SELECT name,
CASE SUBSTRING(name, 1, INSTR(name, ' '))
  WHEN 11 THEN "High School"
  WHEN 10 THEN "High School"
  WHEN 9 THEN "Middle school"
  WHEN 8 THEN "Middle school"
  WHEN 7 THEN "Middle school"
  WHEN 6 THEN "Middle school"
  WHEN 5 THEN "Middle school"
  ELSE "Elementary school"
END AS stage
FROM Class
```

| name | stage             |
| ---- | ----------------- |
| 8 A  | Middle school     |
| 8 B  | Middle school     |
| 9 C  | Middle school     |
| 9 B  | Middle school     |
| 9 A  | Middle school     |
| 10 B | High School       |
| 10 A | High School       |
| 11 B | High School       |
| 11 A | High School       |
| 7 A  | Middle school     |
| 7 B  | Middle school     |
| 6 A  | Middle school     |
| 6 B  | Middle school     |
| 5 A  | Middle school     |
| 5 B  | Middle school     |
| 4 A  | Elementary school |

### Test yourself

What value will the `CASE` operator return in this case?

```sql
CASE 2
  WHEN 0 THEN "Zero"
  WHEN 1 THEN "One"
  ELSE "Many"
END
```
