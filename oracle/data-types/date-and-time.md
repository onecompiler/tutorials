# Oracle Date and Time Data Types

## 1. DATE
Stores date and time information (century, year, month, day, hour, minute, second).

- **Format**: Internally stored as 7 bytes
- **Range**: January 1, 4712 BC to December 31, 9999 AD
- **Precision**: To the second (no fractional seconds)
- **Default format**: Determined by NLS_DATE_FORMAT parameter

### Examples:
```sql
-- Creating a table with DATE
CREATE TABLE orders (
    order_id NUMBER(10),
    order_date DATE,
    ship_date DATE,
    delivery_date DATE
);

-- Inserting dates
INSERT INTO orders VALUES (
    1,
    DATE '2024-01-15',                    -- ANSI date literal
    TO_DATE('20-JAN-2024', 'DD-MON-YYYY'), -- Using TO_DATE
    SYSDATE                                -- Current date and time
);

-- Date arithmetic
SELECT 
    order_date,
    order_date + 7 AS week_later,         -- Add 7 days
    order_date - 1 AS day_before,         -- Subtract 1 day
    order_date + 1/24 AS hour_later       -- Add 1 hour (1/24 of a day)
FROM orders;
```

## 2. TIMESTAMP[(fractional_seconds_precision)]
Extension of DATE that includes fractional seconds.

- **Fractional seconds precision**: 0 to 9 (default is 6)
- **Storage**: 7 or 11 bytes
- **Range**: Same as DATE but with microsecond precision

### Examples:
```sql
-- Different precisions
CREATE TABLE events (
    event_id NUMBER(10),
    event_time TIMESTAMP,          -- Default precision (6)
    precise_time TIMESTAMP(9),     -- Nanosecond precision
    coarse_time TIMESTAMP(3)       -- Millisecond precision
);

-- Inserting timestamps
INSERT INTO events VALUES (
    1,
    TIMESTAMP '2024-01-15 14:30:45.123456',
    SYSTIMESTAMP,
    CURRENT_TIMESTAMP
);

-- Timestamp literals
SELECT TIMESTAMP '2024-01-15 14:30:45.123' FROM dual;
```

## 3. TIMESTAMP WITH TIME ZONE
Stores timestamp with explicit time zone information.

- **Format**: Includes time zone offset or time zone region
- **Storage**: 13 bytes
- **Use cases**: Global applications, different time zones

### Examples:
```sql
CREATE TABLE global_events (
    event_id NUMBER(10),
    event_time TIMESTAMP WITH TIME ZONE
);

-- Inserting with time zones
INSERT INTO global_events VALUES (
    1,
    TIMESTAMP '2024-01-15 14:30:45.123 -05:00'  -- EST
);

INSERT INTO global_events VALUES (
    2,
    TIMESTAMP '2024-01-15 14:30:45.123 America/New_York'
);

-- Converting between time zones
SELECT 
    event_time,
    event_time AT TIME ZONE 'UTC' AS utc_time,
    event_time AT TIME ZONE 'Asia/Tokyo' AS tokyo_time
FROM global_events;
```

## 4. TIMESTAMP WITH LOCAL TIME ZONE
Stores timestamp normalized to database time zone, displays in session time zone.

- **Storage**: 7 or 11 bytes (no time zone stored)
- **Behavior**: Automatically adjusts to session time zone
- **Use cases**: Applications where users are in different time zones

### Examples:
```sql
CREATE TABLE user_sessions (
    session_id NUMBER(10),
    login_time TIMESTAMP WITH LOCAL TIME ZONE,
    last_activity TIMESTAMP WITH LOCAL TIME ZONE
);

-- Data is stored in database time zone but displayed in session time zone
ALTER SESSION SET TIME_ZONE = 'America/New_York';
INSERT INTO user_sessions VALUES (1, CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);

ALTER SESSION SET TIME_ZONE = 'Europe/London';
SELECT * FROM user_sessions; -- Shows times in London time zone
```

## 5. INTERVAL YEAR TO MONTH
Stores a period of time in years and months.

- **Format**: INTERVAL YEAR[(year_precision)] TO MONTH
- **Year precision**: 0 to 9 (default is 2)
- **Use cases**: Age calculations, subscription periods

### Examples:
```sql
CREATE TABLE subscriptions (
    subscription_id NUMBER(10),
    start_date DATE,
    duration INTERVAL YEAR TO MONTH
);

-- Interval literals
INSERT INTO subscriptions VALUES (
    1,
    DATE '2024-01-01',
    INTERVAL '1-6' YEAR TO MONTH  -- 1 year, 6 months
);

-- Interval arithmetic
SELECT 
    start_date,
    start_date + duration AS end_date,
    INTERVAL '2-3' YEAR TO MONTH + INTERVAL '1-6' YEAR TO MONTH AS total
FROM subscriptions;
```

## 6. INTERVAL DAY TO SECOND
Stores a period of time in days, hours, minutes, and seconds.

- **Format**: INTERVAL DAY[(day_precision)] TO SECOND[(fractional_seconds_precision)]
- **Day precision**: 0 to 9 (default is 2)
- **Use cases**: Duration tracking, time differences

### Examples:
```sql
CREATE TABLE time_tracking (
    task_id NUMBER(10),
    duration INTERVAL DAY TO SECOND
);

-- Interval literals
INSERT INTO time_tracking VALUES (
    1,
    INTERVAL '3 12:30:45.123' DAY TO SECOND  -- 3 days, 12 hours, 30 min, 45.123 sec
);

-- Calculating intervals
SELECT 
    TIMESTAMP '2024-01-15 10:00:00' - TIMESTAMP '2024-01-12 08:30:00' AS time_diff
FROM dual;
```

## Date/Time Functions

Oracle provides extensive date/time manipulation functions:
```sql
-- Current date/time
SELECT 
    SYSDATE,                          -- Current date and time
    CURRENT_DATE,                     -- Session date
    SYSTIMESTAMP,                     -- Current timestamp with time zone
    CURRENT_TIMESTAMP,                -- Session timestamp
    LOCALTIMESTAMP                    -- Session timestamp without time zone
FROM dual;

-- Extraction functions
SELECT 
    EXTRACT(YEAR FROM SYSDATE) AS year,
    EXTRACT(MONTH FROM SYSDATE) AS month,
    EXTRACT(DAY FROM SYSDATE) AS day,
    EXTRACT(HOUR FROM SYSTIMESTAMP) AS hour,
    EXTRACT(MINUTE FROM SYSTIMESTAMP) AS minute
FROM dual;

-- Formatting
SELECT 
    TO_CHAR(SYSDATE, 'DD-MON-YYYY HH24:MI:SS') AS formatted_date,
    TO_CHAR(SYSDATE, 'Day, Month DD, YYYY') AS long_format,
    TO_CHAR(SYSDATE, 'IW') AS week_number,
    TO_CHAR(SYSDATE, 'Q') AS quarter
FROM dual;

-- Date arithmetic
SELECT 
    ADD_MONTHS(SYSDATE, 3) AS three_months_later,
    LAST_DAY(SYSDATE) AS end_of_month,
    NEXT_DAY(SYSDATE, 'MONDAY') AS next_monday,
    ROUND(SYSDATE, 'MONTH') AS rounded_to_month,
    TRUNC(SYSDATE, 'YEAR') AS start_of_year
FROM dual;
```

## Best Practices

1. **Use TIMESTAMP** when you need fractional seconds precision
2. **Use TIMESTAMP WITH TIME ZONE** for global applications
3. **Store UTC times** and convert for display when dealing with multiple time zones
4. **Use INTERVAL** types for storing durations and time periods
5. **Be careful with DATE arithmetic** - adding 1 adds one day, not one second
6. **Set appropriate NLS parameters** for consistent date formatting
