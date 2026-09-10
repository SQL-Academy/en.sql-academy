---
meta:
    title: "Partitions in window functions"
    description: "Partitions in SQL window functions. Using partitions across multiple columns. Partition syntax."
---

# Partitions in SQL Window Functions

In the [previous article](https://sql-academy.org/en/guide/windows-functions), we briefly mentioned what partitions
are and how to use them in window functions. Now it's time to dive deeper into partitions 🤓.

## Understanding partitions

> Partitions are subsets of rows that are defined for a window function based on one or more columns in a table.

They are used to segment the data, allowing for more detailed analysis and calculations such as aggregation or ranking within each group.

By partitioning, for example, based on the type of housing in a table with housing price data, we can calculate the average price for each type of housing in a separate column.

![Partitioning schema](https://sql-academy.org/static/guidePage/windows-functions/3_en.png "Partitioning schema")

## Applying partitions in SQL

To use a partition with a window function, you need to follow the following syntax:

```sql
SELECT <window_function>(<table_field>)
OVER (
    PARTITION BY <partition_columns>
)
```

### Example usage

Now let's look at an example of using a partition with a window function using a simple example.

Airbnb database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/partitions).

Consider the `Rooms` table from the `Airbnb` database, specifically the `home_type` and `price` fields:

```sql
SELECT home_type, price FROM Rooms;
```

| home_type       | price |
| --------------- | ----- |
| Private room    | 149   |
| Entire home/apt | 225   |
| Private room    | 150   |
| Entire home/apt | 89    |
| Entire home/apt | 80    |
| Entire home/apt | 200   |
| Private room    | 60    |
| Private room    | 79    |
| Private room    | 79    |
| Entire home/apt | 150   |
| Entire home/apt | 135   |
| Private room    | 85    |
| Private room    | 89    |
| Private room    | 85    |
| Entire home/apt | 120   |
| Entire home/apt | 140   |
| Entire home/apt | 215   |
| Private room    | 140   |
| Entire home/apt | 99    |
| Entire home/apt | 190   |
| Entire home/apt | 299   |
| Private room    | 130   |
| Private room    | 80    |
| Private room    | 110   |
| Entire home/apt | 120   |
| Private room    | 60    |
| Private room    | 80    |
| Entire home/apt | 150   |
| Private room    | 44    |
| Entire home/apt | 180   |
| Private room    | 50    |
| Private room    | 52    |
| Private room    | 55    |
| Private room    | 50    |
| Private room    | 70    |
| Private room    | 89    |
| Private room    | 35    |
| Entire home/apt | 85    |
| Private room    | 150   |
| Shared room     | 40    |
| Private room    | 68    |
| Entire home/apt | 120   |
| Private room    | 120   |
| Private room    | 135   |
| Entire home/apt | 150   |
| Entire home/apt | 150   |
| Private room    | 130   |
| Entire home/apt | 110   |
| Entire home/apt | 115   |
| Private room    | 80    |

We can see that all rental homes are divided into 3 categories: "Private room," "Entire home/apt,"
and "Shared room."

Each category of housing has its own price range. To find out the average price within a specific category and compare it to the current price, we can use window functions.

Let's add another column, `avg_price`, to our result table that calculates the average price per category. It will look like this:

```sql
SELECT
    home_type, price,
    AVG(price) OVER (PARTITION BY home_type) AS avg_price
FROM Rooms
```

| home_type       | price | avg_price |
| --------------- | ----- | --------- |
| Entire home/apt | 225   | 148.6667  |
| Entire home/apt | 180   | 148.6667  |
| Entire home/apt | 150   | 148.6667  |
| Entire home/apt | 85    | 148.6667  |
| Entire home/apt | 120   | 148.6667  |
| Entire home/apt | 120   | 148.6667  |
| Entire home/apt | 299   | 148.6667  |
| Entire home/apt | 190   | 148.6667  |
| Entire home/apt | 99    | 148.6667  |
| Entire home/apt | 215   | 148.6667  |
| Entire home/apt | 140   | 148.6667  |
| Entire home/apt | 120   | 148.6667  |
| Entire home/apt | 150   | 148.6667  |
| Entire home/apt | 135   | 148.6667  |
| Entire home/apt | 150   | 148.6667  |
| Entire home/apt | 110   | 148.6667  |
| Entire home/apt | 115   | 148.6667  |
| Entire home/apt | 200   | 148.6667  |
| Entire home/apt | 150   | 148.6667  |
| Entire home/apt | 80    | 148.6667  |
| Entire home/apt | 89    | 148.6667  |
| Private room    | 68    | 89.4286   |
| Private room    | 50    | 89.4286   |
| Private room    | 70    | 89.4286   |
| Private room    | 80    | 89.4286   |
| Private room    | 89    | 89.4286   |
| Private room    | 149   | 89.4286   |
| Private room    | 35    | 89.4286   |
| Private room    | 150   | 89.4286   |
| Private room    | 130   | 89.4286   |
| Private room    | 120   | 89.4286   |
| Private room    | 135   | 89.4286   |
| Private room    | 130   | 89.4286   |
| Private room    | 150   | 89.4286   |
| Private room    | 60    | 89.4286   |
| Private room    | 79    | 89.4286   |
| Private room    | 79    | 89.4286   |
| Private room    | 85    | 89.4286   |
| Private room    | 89    | 89.4286   |
| Private room    | 85    | 89.4286   |
| Private room    | 140   | 89.4286   |
| Private room    | 55    | 89.4286   |
| Private room    | 80    | 89.4286   |
| Private room    | 110   | 89.4286   |
| Private room    | 60    | 89.4286   |
| Private room    | 80    | 89.4286   |
| Private room    | 44    | 89.4286   |
| Private room    | 50    | 89.4286   |
| Private room    | 52    | 89.4286   |
| Shared room     | 40    | 40        |

What's happening in the added line?

- `PARTITION BY home_type` divides all records into different partitions based on the unique values in the `home_type` column.
- Then, for each record, `AVG(price)` calculates the average price (`price`) within its partition (`home_type`).

The result of executing this part of the query will be the `avg_price` column, which indicates the average price for each record's housing category (`home_type`).

## Partitions on multiple columns

Partitioning can also be done on multiple columns, allowing for more complex and precise segmentation for analysis.

For example, for our `Rooms` table, we can create partitions based on two columns: the housing category `home_type` and the presence of a TV in the accommodation `has_tv`.

Here's an example query with partitioning on two columns:

```sql
SELECT
    home_type, has_tv, price,
    AVG(price) OVER (PARTITION BY home_type, has_tv) AS avg_price
    FROM Rooms
```

| home_type       | has_tv | price | avg_price |
| --------------- | ------ | ----- | --------- |
| Entire home/apt | 0      | 225   | 170       |
| Entire home/apt | 0      | 180   | 170       |
| Entire home/apt | 0      | 80    | 170       |
| Entire home/apt | 0      | 200   | 170       |
| Entire home/apt | 0      | 150   | 170       |
| Entire home/apt | 0      | 150   | 170       |
| Entire home/apt | 0      | 190   | 170       |
| Entire home/apt | 0      | 215   | 170       |
| Entire home/apt | 0      | 140   | 170       |
| Entire home/apt | 1      | 99    | 132.6667  |
| Entire home/apt | 1      | 85    | 132.6667  |
| Entire home/apt | 1      | 150   | 132.6667  |
| Entire home/apt | 1      | 120   | 132.6667  |
| Entire home/apt | 1      | 120   | 132.6667  |
| Entire home/apt | 1      | 299   | 132.6667  |
| Entire home/apt | 1      | 120   | 132.6667  |
| Entire home/apt | 1      | 135   | 132.6667  |
| Entire home/apt | 1      | 150   | 132.6667  |
| Entire home/apt | 1      | 110   | 132.6667  |
| Entire home/apt | 1      | 89    | 132.6667  |
| Entire home/apt | 1      | 115   | 132.6667  |
| Private room    | 0      | 85    | 78.5455   |
| Private room    | 0      | 35    | 78.5455   |
| Private room    | 0      | 150   | 78.5455   |
| Private room    | 0      | 55    | 78.5455   |
| Private room    | 0      | 52    | 78.5455   |
| Private room    | 0      | 50    | 78.5455   |
| Private room    | 0      | 68    | 78.5455   |
| Private room    | 0      | 60    | 78.5455   |
| Private room    | 0      | 135   | 78.5455   |
| Private room    | 0      | 85    | 78.5455   |
| Private room    | 0      | 89    | 78.5455   |
| Private room    | 1      | 120   | 96.4706   |
| Private room    | 1      | 80    | 96.4706   |
| Private room    | 1      | 149   | 96.4706   |
| Private room    | 1      | 130   | 96.4706   |
| Private room    | 1      | 89    | 96.4706   |
| Private room    | 1      | 70    | 96.4706   |
| Private room    | 1      | 50    | 96.4706   |
| Private room    | 1      | 44    | 96.4706   |
| Private room    | 1      | 80    | 96.4706   |
| Private room    | 1      | 60    | 96.4706   |
| Private room    | 1      | 110   | 96.4706   |
| Private room    | 1      | 80    | 96.4706   |
| Private room    | 1      | 130   | 96.4706   |
| Private room    | 1      | 140   | 96.4706   |
| Private room    | 1      | 79    | 96.4706   |
| Private room    | 1      | 79    | 96.4706   |
| Private room    | 1      | 150   | 96.4706   |
| Shared room     | 1      | 40    | 40        |

Here, `PARTITION BY home_type, has_tv` creates unique partitions for each combination of `home_type` and `has_tv`, allowing us to calculate the average price of housing for the current housing category with or without a TV.

![Partitions on two columns](https://sql-academy.org/static/guidePage/partitions/2-columns-partition_en.png "Partitions on two columns")
