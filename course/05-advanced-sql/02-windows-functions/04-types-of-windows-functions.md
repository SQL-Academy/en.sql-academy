---
meta:
    title: "Fundamental window functions"
    description: "Fundamental window functions in SQL - aggregate, ranking, and offset functions. Learn how to use aggregate functions such as SUM, COUNT, AVG, MAX, and MIN to calculate total values. Ranking functions like ROW_NUMBER, RANK, and DENSE_RANK allow for ranking values within a window. Offset functions like LAG, LEAD, FIRST_VALUE, and LAST_VALUE enable access to data from previous and following rows of the window."
---

# Fundamental window functions

In previous articles, we have examined how window functions work and introduced the concept of a data window,
which is passed to the window function. Now it's time to look at the types of window functions available.

## Types of window functions

![categories of window functions](https://sql-academy.org/static/guidePage/types-of-windows-functions/categories_of_windows_functions_en.png "categories of window functions")

Window functions can be divided into 3 groups:

- Aggregate window functions
- Ranking window functions
- Offset window functions

### Aggregate window functions

Aggregate functions are those that perform arithmetic calculations on a data set and return a total value.

- `SUM` — calculates the total sum of values;
- `COUNT` — counts the total number of records in a column (NULL values are not counted);
- `AVG` — calculates the arithmetic mean;
- `MAX` — finds the highest value;
- `MIN` — determines the lowest value.

```sql
SELECT id,
	home_type,
	price,
	SUM(price) OVER(PARTITION BY home_type) AS "Sum",
	COUNT(price) OVER(PARTITION BY home_type) AS "Count",
	AVG(price) OVER(PARTITION BY home_type) AS "Avg",
	MAX(price) OVER(PARTITION BY home_type) AS "Max",
	MIN(price) OVER(PARTITION BY home_type) AS "Min"
FROM Rooms;
```

| id  | home_type       | price | Sum  | Count | Avg      | Max | Min |
| --- | --------------- | ----- | ---- | ----- | -------- | --- | --- |
| 2   | Entire home/apt | 225   | 3122 | 21    | 148.6667 | 299 | 80  |
| 30  | Entire home/apt | 180   | 3122 | 21    | 148.6667 | 299 | 80  |
| 28  | Entire home/apt | 150   | 3122 | 21    | 148.6667 | 299 | 80  |
| 38  | Entire home/apt | 85    | 3122 | 21    | 148.6667 | 299 | 80  |
| 25  | Entire home/apt | 120   | 3122 | 21    | 148.6667 | 299 | 80  |
| 42  | Entire home/apt | 120   | 3122 | 21    | 148.6667 | 299 | 80  |
| 21  | Entire home/apt | 299   | 3122 | 21    | 148.6667 | 299 | 80  |
| 20  | Entire home/apt | 190   | 3122 | 21    | 148.6667 | 299 | 80  |
| 19  | Entire home/apt | 99    | 3122 | 21    | 148.6667 | 299 | 80  |
| 17  | Entire home/apt | 215   | 3122 | 21    | 148.6667 | 299 | 80  |
| 16  | Entire home/apt | 140   | 3122 | 21    | 148.6667 | 299 | 80  |
| 15  | Entire home/apt | 120   | 3122 | 21    | 148.6667 | 299 | 80  |
| 46  | Entire home/apt | 150   | 3122 | 21    | 148.6667 | 299 | 80  |
| 11  | Entire home/apt | 135   | 3122 | 21    | 148.6667 | 299 | 80  |
| 10  | Entire home/apt | 150   | 3122 | 21    | 148.6667 | 299 | 80  |
| 48  | Entire home/apt | 110   | 3122 | 21    | 148.6667 | 299 | 80  |
| 49  | Entire home/apt | 115   | 3122 | 21    | 148.6667 | 299 | 80  |
| 6   | Entire home/apt | 200   | 3122 | 21    | 148.6667 | 299 | 80  |
| 45  | Entire home/apt | 150   | 3122 | 21    | 148.6667 | 299 | 80  |
| 5   | Entire home/apt | 80    | 3122 | 21    | 148.6667 | 299 | 80  |
| 4   | Entire home/apt | 89    | 3122 | 21    | 148.6667 | 299 | 80  |
| 41  | Private room    | 68    | 2504 | 28    | 89.4286  | 150 | 35  |
| 34  | Private room    | 50    | 2504 | 28    | 89.4286  | 150 | 35  |
| 35  | Private room    | 70    | 2504 | 28    | 89.4286  | 150 | 35  |
| 50  | Private room    | 80    | 2504 | 28    | 89.4286  | 150 | 35  |
| 36  | Private room    | 89    | 2504 | 28    | 89.4286  | 150 | 35  |
| 1   | Private room    | 149   | 2504 | 28    | 89.4286  | 150 | 35  |
| 37  | Private room    | 35    | 2504 | 28    | 89.4286  | 150 | 35  |
| 39  | Private room    | 150   | 2504 | 28    | 89.4286  | 150 | 35  |
| 47  | Private room    | 130   | 2504 | 28    | 89.4286  | 150 | 35  |
| 43  | Private room    | 120   | 2504 | 28    | 89.4286  | 150 | 35  |
| 44  | Private room    | 135   | 2504 | 28    | 89.4286  | 150 | 35  |
| 22  | Private room    | 130   | 2504 | 28    | 89.4286  | 150 | 35  |
| 3   | Private room    | 150   | 2504 | 28    | 89.4286  | 150 | 35  |
| 7   | Private room    | 60    | 2504 | 28    | 89.4286  | 150 | 35  |
| 8   | Private room    | 79    | 2504 | 28    | 89.4286  | 150 | 35  |
| 9   | Private room    | 79    | 2504 | 28    | 89.4286  | 150 | 35  |
| 12  | Private room    | 85    | 2504 | 28    | 89.4286  | 150 | 35  |
| 13  | Private room    | 89    | 2504 | 28    | 89.4286  | 150 | 35  |
| 14  | Private room    | 85    | 2504 | 28    | 89.4286  | 150 | 35  |
| 18  | Private room    | 140   | 2504 | 28    | 89.4286  | 150 | 35  |
| 18  | Private room    | 140   | 2504 | 28    | 89.4286  | 150 | 35  |
| 33  | Private room    | 55    | 2504 | 28    | 89.4286  | 150 | 35  |
| 23  | Private room    | 80    | 2504 | 28    | 89.4286  | 150 | 35  |
| 24  | Private room    | 110   | 2504 | 28    | 89.4286  | 150 | 35  |
| 26  | Private room    | 60    | 2504 | 28    | 89.4286  | 150 | 35  |
| 27  | Private room    | 80    | 2504 | 28    | 89.4286  | 150 | 35  |
| 29  | Private room    | 44    | 2504 | 28    | 89.4286  | 150 | 35  |
| 31  | Private room    | 50    | 2504 | 28    | 89.4286  | 150 | 35  |
| 32  | Private room    | 52    | 2504 | 28    | 89.4286  | 150 | 35  |
| 40  | Shared room     | 40    | 40   | 1     | 40       | 40  | 40  |

### Ranking window functions

Ranking window functions are those that rank a value for each row in the window.

In ranking functions, the `OVER` keyword is followed by the mandatory `ORDER BY` condition, which determines the sorting order for ranking.

- `ROW_NUMBER` - returns the row number, used for numbering;
- `RANK` - returns the rank of each row. Here's how it works:
    - Sorting: firstly, rows are sorted by one or more columns. These columns are specified in `ORDER BY` in the `OVER` clause.
    - Assigning ranks: each unique row or group of rows that have the same values in the sorting columns is assigned a rank. The rank starts from 1.
    - Identical values: if several rows have the same values in the sorting columns, they receive the same rank. For example, if two rows are in second place, both receive rank 2.
    - Skipping ranks: after a group of rows with the same rank, the next rank increases by the number of rows in that group. For example, if two rows have rank 2, the next row will get rank 4, not 3.
    - Continuing sorting: this process continues until ranks have been assigned to all rows in the result set.
- `DENSE_RANK` - returns the rank of each row. Unlike the `RANK` function, it doesn't skip ranks and after a group of identical values, the rank increases by one, not by the number of rows. For example, if two rows have rank 2, the next row will get rank 3, not 4.

```sql
SELECT id,
	home_type,
	price,
	ROW_NUMBER() OVER(PARTITION BY home_type ORDER BY price) AS "row_number",
	RANK() OVER(PARTITION BY home_type ORDER BY price) AS "rank",
	DENSE_RANK() OVER(PARTITION BY home_type ORDER BY price) AS "dense_rank"
FROM Rooms;
```

| id  | home_type       | price | row_number | rank | dense_rank |
| --- | --------------- | ----- | ---------- | ---- | ---------- |
| 5   | Entire home/apt | 80    | 1          | 1    | 1          |
| 38  | Entire home/apt | 85    | 2          | 2    | 2          |
| 4   | Entire home/apt | 89    | 3          | 3    | 3          |
| 19  | Entire home/apt | 99    | 4          | 4    | 4          |
| 48  | Entire home/apt | 110   | 5          | 5    | 5          |
| 49  | Entire home/apt | 115   | 6          | 6    | 6          |
| 25  | Entire home/apt | 120   | 7          | 7    | 7          |
| 15  | Entire home/apt | 120   | 8          | 7    | 7          |
| 42  | Entire home/apt | 120   | 9          | 7    | 7          |
| 11  | Entire home/apt | 135   | 10         | 10   | 8          |
| 16  | Entire home/apt | 140   | 11         | 11   | 9          |
| 28  | Entire home/apt | 150   | 12         | 12   | 10         |
| 10  | Entire home/apt | 150   | 13         | 12   | 10         |
| 45  | Entire home/apt | 150   | 14         | 12   | 10         |
| 46  | Entire home/apt | 150   | 15         | 12   | 10         |
| 30  | Entire home/apt | 180   | 16         | 16   | 11         |
| 20  | Entire home/apt | 190   | 17         | 17   | 12         |
| 6   | Entire home/apt | 200   | 18         | 18   | 13         |
| 17  | Entire home/apt | 215   | 19         | 19   | 14         |
| 2   | Entire home/apt | 225   | 20         | 20   | 15         |
| 21  | Entire home/apt | 299   | 21         | 21   | 16         |
| 37  | Private room    | 35    | 1          | 1    | 1          |
| 29  | Private room    | 44    | 2          | 2    | 2          |
| 34  | Private room    | 50    | 3          | 3    | 3          |
| 31  | Private room    | 50    | 4          | 3    | 3          |
| 32  | Private room    | 52    | 5          | 5    | 4          |
| 33  | Private room    | 55    | 6          | 6    | 5          |
| 26  | Private room    | 60    | 7          | 7    | 6          |
| 7   | Private room    | 60    | 8          | 7    | 6          |
| 41  | Private room    | 68    | 9          | 9    | 7          |
| 35  | Private room    | 70    | 10         | 10   | 8          |
| 8   | Private room    | 79    | 11         | 11   | 9          |
| 9   | Private room    | 79    | 12         | 11   | 9          |
| 27  | Private room    | 80    | 13         | 13   | 10         |
| 23  | Private room    | 80    | 14         | 13   | 10         |
| 50  | Private room    | 80    | 15         | 13   | 10         |
| 12  | Private room    | 85    | 16         | 16   | 11         |
| 14  | Private room    | 85    | 17         | 16   | 11         |
| 13  | Private room    | 89    | 18         | 18   | 12         |
| 36  | Private room    | 89    | 19         | 18   | 12         |
| 24  | Private room    | 110   | 20         | 20   | 13         |
| 43  | Private room    | 120   | 21         | 21   | 14         |
| 22  | Private room    | 130   | 22         | 22   | 15         |
| 47  | Private room    | 130   | 23         | 22   | 15         |
| 44  | Private room    | 135   | 24         | 24   | 16         |
| 18  | Private room    | 140   | 25         | 25   | 17         |
| 1   | Private room    | 149   | 26         | 26   | 18         |
| 3   | Private room    | 150   | 27         | 27   | 19         |
| 39  | Private room    | 150   | 28         | 27   | 19         |
| 40  | Shared room     | 40    | 1          | 1    | 1          |

### Offset window functions

Offset window functions are those that allow moving and accessing different rows in the window relative to the current row, as well as accessing values at the beginning or end of the window.

- `LAG` - accesses data from previous rows of the window.

    It has three arguments: the column whose value needs to be returned, the number of rows to offset (default is 1), and the value to return if the offset returns a `NULL` value.

- `LEAD` - accesses data from following rows. Similar to LAG, it has 3 arguments.

- `FIRST_VALUE` - returns the first value in the window. Takes a column as an argument, the value of which needs to be returned.

- `LAST_VALUE` - returns the last value in the window. Takes a column as an argument, the value of which needs to be returned.

    > When `ORDER BY` is used, the default window frame runs from the start of the partition to the current row (`RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`). Because of this, `LAST_VALUE` returns the value from the current row rather than the last row of the entire partition. To get the actual last value of the partition, explicitly extend the frame boundaries: `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`.

```sql
SELECT id,
	home_type,
	price,
	LAG(price) OVER(PARTITION BY home_type ORDER BY price) AS "lag",
	LAG(price, 2) OVER(PARTITION BY home_type ORDER BY price) AS "lag_2",
	LEAD(price) OVER(PARTITION BY home_type ORDER BY price) AS "lead",
	FIRST_VALUE(price) OVER(PARTITION BY home_type ORDER BY price) AS "first_value",
	LAST_VALUE(price) OVER(PARTITION BY home_type ORDER BY price ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS "last_value"
FROM Rooms;
```

| id  | home_type       | price | lag  | lag_2 | lead | first_value | last_value |
| --- | --------------- | ----- | ---- | ----- | ---- | ----------- | ---------- |
| 5   | Entire home/apt | 80    | null | null  | 85   | 80          | 299        |
| 38  | Entire home/apt | 85    | 80   | null  | 89   | 80          | 299        |
| 4   | Entire home/apt | 89    | 85   | 80    | 99   | 80          | 299        |
| 19  | Entire home/apt | 99    | 89   | 85    | 110  | 80          | 299        |
| 48  | Entire home/apt | 110   | 99   | 89    | 115  | 80          | 299        |
| 49  | Entire home/apt | 115   | 110  | 99    | 120  | 80          | 299        |
| 25  | Entire home/apt | 120   | 115  | 110   | 120  | 80          | 299        |
| 15  | Entire home/apt | 120   | 120  | 115   | 120  | 80          | 299        |
| 42  | Entire home/apt | 120   | 120  | 120   | 135  | 80          | 299        |
| 11  | Entire home/apt | 135   | 120  | 120   | 140  | 80          | 299        |
| 16  | Entire home/apt | 140   | 135  | 120   | 150  | 80          | 299        |
| 28  | Entire home/apt | 150   | 140  | 135   | 150  | 80          | 299        |
| 10  | Entire home/apt | 150   | 150  | 140   | 150  | 80          | 299        |
| 45  | Entire home/apt | 150   | 150  | 150   | 150  | 80          | 299        |
| 46  | Entire home/apt | 150   | 150  | 150   | 180  | 80          | 299        |
| 30  | Entire home/apt | 180   | 150  | 150   | 190  | 80          | 299        |
| 20  | Entire home/apt | 190   | 180  | 150   | 200  | 80          | 299        |
| 6   | Entire home/apt | 200   | 190  | 180   | 215  | 80          | 299        |
| 17  | Entire home/apt | 215   | 200  | 190   | 225  | 80          | 299        |
| 2   | Entire home/apt | 225   | 215  | 200   | 299  | 80          | 299        |
| 21  | Entire home/apt | 299   | 225  | 215   | null | 80          | 299        |
| 37  | Private room    | 35    | null | null  | 44   | 35          | 150        |
| 29  | Private room    | 44    | 35   | null  | 50   | 35          | 150        |
| 34  | Private room    | 50    | 44   | 35    | 50   | 35          | 150        |
| 31  | Private room    | 50    | 50   | 44    | 52   | 35          | 150        |
| 32  | Private room    | 52    | 50   | 50    | 55   | 35          | 150        |
| 33  | Private room    | 55    | 52   | 50    | 60   | 35          | 150        |
| 26  | Private room    | 60    | 55   | 52    | 60   | 35          | 150        |
| 7   | Private room    | 60    | 60   | 55    | 68   | 35          | 150        |
| 41  | Private room    | 68    | 60   | 60    | 70   | 35          | 150        |
| 35  | Private room    | 70    | 68   | 60    | 79   | 35          | 150        |
| 8   | Private room    | 79    | 70   | 68    | 79   | 35          | 150        |
| 9   | Private room    | 79    | 79   | 70    | 80   | 35          | 150        |
| 27  | Private room    | 80    | 79   | 79    | 80   | 35          | 150        |
| 23  | Private room    | 80    | 80   | 79    | 80   | 35          | 150        |
| 50  | Private room    | 80    | 80   | 80    | 85   | 35          | 150        |
| 12  | Private room    | 85    | 80   | 80    | 85   | 35          | 150        |
| 14  | Private room    | 85    | 85   | 80    | 89   | 35          | 150        |
| 13  | Private room    | 89    | 85   | 85    | 89   | 35          | 150        |
| 36  | Private room    | 89    | 89   | 85    | 110  | 35          | 150        |
| 24  | Private room    | 110   | 89   | 89    | 120  | 35          | 150        |
| 43  | Private room    | 120   | 110  | 89    | 130  | 35          | 150        |
| 22  | Private room    | 130   | 120  | 110   | 130  | 35          | 150        |
| 47  | Private room    | 130   | 130  | 120   | 135  | 35          | 150        |
| 44  | Private room    | 135   | 130  | 130   | 140  | 35          | 150        |
| 18  | Private room    | 140   | 135  | 130   | 149  | 35          | 150        |
| 1   | Private room    | 149   | 140  | 135   | 150  | 35          | 150        |
| 3   | Private room    | 150   | 149  | 140   | 150  | 35          | 150        |
| 39  | Private room    | 150   | 150  | 149   | null | 35          | 150        |
| 40  | Shared room     | 40    | null | null  | null | 40          | 40         |
