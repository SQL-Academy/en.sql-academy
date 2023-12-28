# Partitions in SQL Window Functions

In the [previous article](https://sql-academy.org//guide/windows-functions), we briefly mentioned what partitions
are and how to use them in window functions. Now it's time to dive deeper into partitions.

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

<ERD databaseName="Airbnb" />

Consider the `Rooms` table from the `Airbnb` database, specifically the `home_type` and `price` fields:

```sql-Airbnb-executable
SELECT home_type, price FROM Rooms;
```

We can see that all rental homes are divided into 3 categories: "Private room," "Entire home/apt,"
and "Shared room."

Each category of housing has its own price range. To find out the average price within a specific category and compare it to the current price, we can use window functions.

Let's add another column, `avg_price`, to our result table that calculates the average price per category. It will look like this:

```sql-Airbnb-executable
SELECT
    home_type, price,
    AVG(price) OVER (PARTITION BY home_type) AS avg_price
    FROM Rooms
```

What's happening in the added line?

- `PARTITION BY home_type` divides all records into different partitions based on the unique values in the `home_type` column.
- Then, for each record, `AVG(price)` calculates the average price (`price`) within its partition (`home_type`).

The result of executing this part of the query will be the `avg_price` column, which indicates the average price for each record's housing category (`home_type`).

## Partitions on multiple columns

Partitioning can also be done on multiple columns, allowing for more complex and precise segmentation for analysis.

For example, for our `Rooms` table, we can create partitions based on two columns: the housing category `home_type` and the presence of a TV in the accommodation `has_tv` .

Here's an example query with partitioning on two columns:

```sql-Airbnb-executable
SELECT
    home_type, has_tv, price,
    AVG(price) OVER (PARTITION BY home_type, has_tv) AS avg_price
    FROM Rooms
```

Here, `PARTITION BY home_type, has_tv` creates unique partitions for each combination of `home_type` and `has_tv`, allowing us to calculate the average price of housing for the current housing category with or without a TV.

![Partitions on two columns](https://sql-academy.org/static/guidePage/partitions/2-columns-partition_en.png "Partitions on two columns")
