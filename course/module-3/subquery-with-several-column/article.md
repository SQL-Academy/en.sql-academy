---
meta:
    title: 'Multi-column subqueries'
    description: 'Multi-column subqueries in SQL, subqueries with arbitrary table and comparison with multiple columns in SQL.'
---

# Multi-column subqueries

So far, we have only considered subqueries that return a single column. However, we can also work with subqueries that return multiple columns and multiple rows (derived tables).

## Comparison with multiple columns

SQL supports comparison not only with a single column, but also allows pairwise comparison of values in the main query with values in the subquery.

For example, if we want to get information about all reservations where the accommodation price at the time of booking (`Reservations.price`) matches the current price of the accommodation (`Rooms.price`), we can do it as follows:

```sql
SELECT * FROM Reservations
    WHERE (room_id, price) IN (SELECT id, price FROM Rooms);
```

| id  | user_id | room_id | start_date               | end_date                 | price | total |
| --- | ------- | ------- | ------------------------ | ------------------------ | ----- | ----- |
| 5   | 17      | 1       | 2019-02-02T12:00:00.000Z | 2019-02-04T12:00:00.000Z | 149   | 298   |
| 6   | 14      | 2       | 2020-03-21T09:00:00.000Z | 2020-03-23T09:00:00.000Z | 225   | 450   |
| 7   | 19      | 13      | 2020-03-21T10:00:00.000Z | 2020-04-21T10:00:00.000Z | 89    | 2679  |
| 8   | 29      | 16      | 2019-06-14T10:00:00.000Z | 2019-06-24T10:00:00.000Z | 140   | 1400  |
| 12  | 7       | 19      | 2020-05-01T10:00:00.000Z | 2020-05-02T10:00:00.000Z | 99    | 99    |
| 14  | 13      | 7       | 2019-09-13T10:00:00.000Z | 2019-09-17T10:00:00.000Z | 60    | 240   |
| 15  | 12      | 5       | 2020-05-14T10:00:00.000Z | 2020-05-15T10:00:00.000Z | 80    | 80    |
| 16  | 10      | 5       | 2020-03-26T10:00:00.000Z | 2020-03-27T10:00:00.000Z | 80    | 80    |
| 17  | 11      | 50      | 2019-11-18T11:00:00.000Z | 2019-11-25T11:00:00.000Z | 80    | 560   |
| 19  | 28      | 49      | 2020-04-01T11:00:00.000Z | 2020-04-02T11:00:00.000Z | 115   | 115   |
| 20  | 1       | 49      | 2020-06-01T10:00:00.000Z | 2020-06-11T10:00:00.000Z | 115   | 1150  |
| 21  | 2       | 48      | 2019-11-07T10:00:00.000Z | 2019-11-10T10:00:00.000Z | 110   | 330   |
| 22  | 12      | 32      | 2020-01-14T13:00:00.000Z | 2020-01-18T13:00:00.000Z | 52    | 208   |
| 23  | 12      | 32      | 2019-09-17T13:00:00.000Z | 2019-09-18T13:00:00.000Z | 52    | 52    |
| 24  | 7       | 19      | 2020-01-08T13:00:00.000Z | 2020-01-11T13:00:00.000Z | 99    | 297   |
| 25  | 18      | 19      | 2019-11-01T10:00:00.000Z | 2019-11-16T10:00:00.000Z | 99    | 1485  |
| 26  | 21      | 17      | 2019-11-03T09:00:00.000Z | 2019-11-05T09:00:00.000Z | 215   | 430   |
| 27  | 31      | 25      | 2020-04-20T09:00:00.000Z | 2020-04-22T09:00:00.000Z | 120   | 240   |
| 28  | 21      | 14      | 2020-02-08T10:00:00Z     | 2020-02-12T10:00:00Z     | 85    | 340   |
| 29  | 21      | 39      | 2019-12-08T10:00:00Z     | 2019-12-09T10:00:00Z     | 150   | 150   |

In this example, the subquery returns a table with the identifiers of residential properties and their current price:

```sql
SELECT id, price FROM Rooms
```

| id  | price |
| --- | ----- |
| 1   | 149   |
| 2   | 225   |
| 3   | 150   |
| 4   | 89    |
| 5   | 80    |
| 6   | 200   |
| 7   | 60    |
| 8   | 79    |
| 9   | 79    |
| 10  | 150   |
| 11  | 135   |
| 12  | 85    |
| 13  | 89    |
| 14  | 85    |
| 15  | 120   |
| 16  | 140   |
| 17  | 215   |
| 18  | 140   |
| 19  | 99    |
| 20  | 190   |
| 21  | 299   |
| 22  | 130   |
| 23  | 80    |
| 24  | 110   |
| 25  | 120   |
| 26  | 60    |
| 27  | 80    |
| 28  | 150   |
| 29  | 44    |
| 30  | 180   |
| 31  | 50    |
| 32  | 52    |
| 33  | 55    |
| 34  | 50    |
| 35  | 70    |
| 36  | 89    |
| 37  | 35    |
| 38  | 85    |
| 39  | 150   |
| 40  | 40    |
| 41  | 68    |
| 42  | 120   |
| 43  | 120   |
| 44  | 135   |
| 45  | 150   |
| 46  | 150   |
| 47  | 130   |
| 48  | 110   |
| 49  | 115   |
| 50  | 80    |

And then, using this table, we limit all bookings only to those where the pair of values `room_id` and `price` can be found in the subquery table.

The same solution can be applied without the need for comparison across multiple columns, although it would be less concise:

```sql
SELECT Reservations.* FROM Reservations
INNER JOIN Rooms
ON Reservations.room_id = Rooms.id
WHERE Reservations.price = Rooms.price;
```
