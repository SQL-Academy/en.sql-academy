---
meta:
    title: 'SQL Event Scheduler: MySQL EVENT & PostgreSQL pg_cron Guide'
    description: 'Complete guide to creating automated tasks in MySQL and PostgreSQL. Learn to use EVENT scheduler and pg_cron for data cleanup, statistics updates, and report generation on schedule. Code examples and practical tips included.'
---

<DBMSSwitcher />

# Scheduled Events

In real-world applications, there's often a need to automatically execute certain actions on a schedule: cleaning up old records, updating statistics, generating reports. SQL provides a mechanism called **scheduled events** for these tasks.

> **Event** is a task the database runs for you on a schedule. You set it up — it runs automatically.

<MySQLOnly>

Events in MySQL are similar to a task scheduler in an operating system: you create a task once, and the database executes it automatically on schedule.

</MySQLOnly>

<PostgreSQLOnly>

In PostgreSQL, automatic task execution is handled by the **pg_cron** extension. This extension allows you to schedule SQL commands using cron syntax (like in Unix systems).

</PostgreSQLOnly>

## When it is useful

Scheduled events help automate the following tasks:

-   **Data cleanup**: removing outdated log records or temporary data
-   **Statistics updates**: recalculating aggregated data for analytics
-   **Report generation**: automatically creating periodic reports
-   **Backups**: creating copies of important data

## Enabling the scheduler

<MySQLOnly>

Before creating events, make sure the event scheduler is enabled:

```sql
SHOW VARIABLES LIKE 'event_scheduler';
```

If the scheduler is disabled, enable it:

```sql
SET GLOBAL event_scheduler = ON;
```

</MySQLOnly>

<PostgreSQLOnly>

To use scheduled tasks in PostgreSQL, you need to install the pg_cron extension:

```sql
CREATE EXTENSION IF NOT EXISTS pg_cron;
```

> **Important:** The pg_cron extension may require superuser privileges and additional PostgreSQL configuration. In cloud services (AWS RDS, Azure), it may already be pre-installed.

</PostgreSQLOnly>

## Creating a One-Time Event

Let's start with the simplest case — an event that executes once at a specific time:

<MySQLOnly>

```sql-executable
CREATE EVENT cleanup_old_logs
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 DAY
DO
    DELETE FROM logs WHERE created_at < NOW() - INTERVAL 30 DAY;
```

This event will delete log records older than 30 days, 24 hours after the event is created.

**Breaking down the syntax:**

-   `CREATE EVENT cleanup_old_logs` — create an event named `cleanup_old_logs`
-   `ON SCHEDULE AT` — specify when the event should execute
-   `CURRENT_TIMESTAMP + INTERVAL 1 DAY` — execution time (in 1 day)
-   `DO` — the code to execute (any SQL statement)

</MySQLOnly>

<PostgreSQLOnly>

```sql
SELECT cron.schedule(
    'cleanup_old_logs',
    '0 3 * * *',
    'DELETE FROM logs WHERE created_at < NOW() - INTERVAL ''30 days'''
);
```

This event will run every day at 3:00 AM and delete log records older than 30 days.

**Breaking down the syntax:**

-   `cron.schedule()` — function to create a scheduled task
-   `'cleanup_old_logs'` — task name
-   `'0 3 * * *'` — schedule in cron format (minute hour day month day_of_week)
-   Last parameter — SQL command to execute

**Cron schedule format:**

![Format cron scheduler](https://sql-academy.org/static/guidePage/scheduled-events/cron_schedule_en.png 'Format cron scheduler')

</PostgreSQLOnly>

## Creating a Recurring Event

More often, events need to run periodically — every day, hour, or minute:

<MySQLOnly>

```sql-executable
CREATE EVENT update_statistics
ON SCHEDULE EVERY 1 HOUR
DO
BEGIN
    UPDATE product_stats SET
        total_sales = (SELECT SUM(amount) FROM orders WHERE product_id = product_stats.product_id),
        last_updated = NOW();
END;
```

This event will update sales statistics every hour.

**Breaking down the syntax:**

-   `ON SCHEDULE EVERY 1 HOUR` — execute every hour
-   `BEGIN ... END` — block of multiple SQL statements

**Interval options:**

-   `EVERY 1 MINUTE` — every minute
-   `EVERY 1 HOUR` — every hour
-   `EVERY 1 DAY` — every day
-   `EVERY 1 WEEK` — every week
-   `EVERY 1 MONTH` — every month
-   `EVERY 30 SECOND` — every 30 seconds

</MySQLOnly>

<PostgreSQLOnly>

```sql
SELECT cron.schedule(
    'update_statistics_hourly',
    '0 * * * *',
    $$
    UPDATE product_stats SET
        total_sales = (SELECT SUM(amount) FROM orders WHERE product_id = product_stats.product_id),
        last_updated = NOW()
    $$
);
```

This event will update sales statistics every hour (at the start of each hour).

**Schedule examples:**

-   `'*/5 * * * *'` — every 5 minutes
-   `'0 * * * *'` — every hour (at the start of the hour)
-   `'0 0 * * *'` — every day at midnight
-   `'0 0 * * 0'` — every Sunday at midnight
-   `'0 9 1 * *'` — first day of each month at 9:00 AM

</PostgreSQLOnly>

## Event with Limited Duration

Sometimes you need an event to work only during a specific period:

<MySQLOnly>

```sql-executable
CREATE EVENT seasonal_discount
ON SCHEDULE EVERY 1 DAY
STARTS '2025-12-01 00:00:00'
ENDS '2025-12-31 23:59:59'
DO
    UPDATE products SET price = price * 0.9 WHERE category = 'seasonal';
```

This event will apply a 10% discount to seasonal products every day during December 2025.

**New elements:**

-   `STARTS` — start of the event's active period
-   `ENDS` — end of the event's active period

After the specified date, the event will automatically stop executing.

</MySQLOnly>

<PostgreSQLOnly>

Pg_cron doesn't have built-in support for automatic task termination, but you can include date checking in the command itself:

```sql
SELECT cron.schedule(
    'seasonal_discount',
    '0 0 * * *',
    $$
    UPDATE products
    SET price = price * 0.9
    WHERE category = 'seasonal'
      AND CURRENT_DATE BETWEEN '2025-12-01' AND '2025-12-31'
    $$
);
```

Alternatively, you can create a task to remove the event at the end of the period:

```sql
-- Create a task to remove the event
SELECT cron.schedule(
    'remove_seasonal_discount',
    '0 0 1 1 *',  -- January 1st at midnight
    $$SELECT cron.unschedule('seasonal_discount')$$
);
```

</PostgreSQLOnly>

## Viewing Existing Events

<MySQLOnly>

To see all created events:

```sql
SHOW EVENTS;
```

To view events in a specific database:

```sql
SHOW EVENTS FROM your_database_name;
```

</MySQLOnly>

<PostgreSQLOnly>

To see all scheduled tasks:

```sql
SELECT * FROM cron.job;
```

This will return a table with all tasks, including their schedule and commands.

To view task execution history:

```sql
SELECT * FROM cron.job_run_details
ORDER BY start_time DESC
LIMIT 10;
```

</PostgreSQLOnly>

## Managing Events

<MySQLOnly>

**Temporarily disable an event:**

```sql
ALTER EVENT cleanup_old_logs DISABLE;
```

**Enable an event:**

```sql
ALTER EVENT cleanup_old_logs ENABLE;
```

**Change event schedule:**

```sql
ALTER EVENT cleanup_old_logs
ON SCHEDULE EVERY 2 HOUR;
```

**Delete an event:**

```sql
DROP EVENT IF EXISTS cleanup_old_logs;
```

</MySQLOnly>

<PostgreSQLOnly>

**Remove a scheduled task:**

```sql
SELECT cron.unschedule('cleanup_old_logs');
```

Or by job ID:

```sql
SELECT cron.unschedule(42);  -- where 42 is the jobid from cron.job table
```

**Modify a task:**

In pg_cron, you can't modify an existing task directly. You need to remove the old one and create a new one:

```sql
-- Remove the old one
SELECT cron.unschedule('cleanup_old_logs');

-- Create a new one with updated schedule
SELECT cron.schedule(
    'cleanup_old_logs',
    '0 */2 * * *',  -- Every 2 hours
    'DELETE FROM logs WHERE created_at < NOW() - INTERVAL ''30 days'''
);
```

</PostgreSQLOnly>

## Important Considerations When Working with Events

<MySQLOnly>

1. **Access privileges**: Creating events requires the `EVENT` privilege.

2. **Time zone**: Events execute according to the database server's time zone.

3. **Performance**: Avoid creating events with very short intervals (every minute), as this can impact performance.

</MySQLOnly>

<PostgreSQLOnly>

1. **Access privileges**: Using pg_cron typically requires superuser privileges or special configuration.

2. **Time zone**: Pg_cron tasks execute according to PostgreSQL's time zone (check with `SHOW timezone;`).

3. **Performance**: Pg_cron checks the schedule every second, so the minimum precision is 1 minute.

4. **Logging**: All task executions are saved in the `cron.job_run_details` table, which is useful for debugging.

</PostgreSQLOnly>

## Self-Check

What is the minimum interval you can use for recurring events?


Scheduled events are a powerful tool for automating routine database tasks. They help maintain data cleanliness, update statistics, and perform maintenance operations without developer intervention! 🚀
