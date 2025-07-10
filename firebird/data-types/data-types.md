# Firebird Data Types

Firebird supports a comprehensive set of SQL standard data types. Understanding these data types is essential for designing efficient database schemas and ensuring data integrity.

## Numeric Data Types

### Integer Types

| Data Type | Storage | Range | Description |
|-----------|---------|--------|-------------|
| SMALLINT | 2 bytes | -32,768 to 32,767 | Small integer |
| INTEGER | 4 bytes | -2,147,483,648 to 2,147,483,647 | Standard integer |
| BIGINT | 8 bytes | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | Large integer |
| INT128 | 16 bytes | -170,141,183,460,469,231,731,687,303,715,884,105,728 to 170,141,183,460,469,231,731,687,303,715,884,105,727 | Very large integer (Firebird 4.0+) |

### Exact Numeric Types

| Data Type | Storage | Description |
|-----------|---------|-------------|
| NUMERIC(p,s) | Variable | Exact numeric with precision p and scale s |
| DECIMAL(p,s) | Variable | Synonym for NUMERIC |
| DECFLOAT(16) | 8 bytes | Decimal floating-point, 16 digits precision (Firebird 4.0+) |
| DECFLOAT(34) | 16 bytes | Decimal floating-point, 34 digits precision (Firebird 4.0+) |

- **Precision (p)**: Total number of digits (1-38)
- **Scale (s)**: Number of digits after decimal point (0-38)
- Default: NUMERIC(9,0)

Examples:
```sql
CREATE TABLE products (
    price NUMERIC(10,2),      -- e.g., 99999999.99
    tax_rate DECIMAL(5,4),    -- e.g., 0.1875
    quantity INTEGER
);
```

### Approximate Numeric Types

| Data Type | Storage | Range | Description |
|-----------|---------|--------|-------------|
| FLOAT | 4 bytes | ~1.175E-38 to ~3.402E+38 | Single precision floating-point |
| REAL | 4 bytes | Same as FLOAT | Synonym for FLOAT |
| DOUBLE PRECISION | 8 bytes | ~2.225E-308 to ~1.797E+308 | Double precision floating-point |

## Character String Types

### Fixed-Length Strings

| Data Type | Maximum Size | Description |
|-----------|--------------|-------------|
| CHAR(n) | 32,767 bytes | Fixed-length, blank-padded |
| CHARACTER(n) | 32,767 bytes | Synonym for CHAR |
| NCHAR(n) | 32,767 bytes | Fixed-length, uses default character set |

### Variable-Length Strings

| Data Type | Maximum Size | Description |
|-----------|--------------|-------------|
| VARCHAR(n) | 32,765 bytes | Variable-length string |
| CHARACTER VARYING(n) | 32,765 bytes | Synonym for VARCHAR |
| NCHAR VARYING(n) | 32,765 bytes | Variable-length, uses default character set |

Examples:
```sql
CREATE TABLE users (
    user_code CHAR(10),           -- Always 10 characters
    username VARCHAR(50),         -- Up to 50 characters
    full_name VARCHAR(100),       -- Up to 100 characters
    bio VARCHAR(8000)            -- Longer text
);
```

### Character Set and Collation

```sql
CREATE TABLE international (
    name_utf8 VARCHAR(50) CHARACTER SET UTF8,
    name_latin VARCHAR(50) CHARACTER SET ISO8859_1,
    name_unicode VARCHAR(50) CHARACTER SET UNICODE_FSS COLLATE UNICODE_CI
);
```

## Date and Time Types

| Data Type | Storage | Range | Description |
|-----------|---------|--------|-------------|
| DATE | 4 bytes | 01.01.0001 to 31.12.9999 | Date only |
| TIME | 4 bytes | 00:00:00.0000 to 23:59:59.9999 | Time only (no timezone) |
| TIME WITH TIME ZONE | 6 bytes | Time with timezone info | Firebird 4.0+ |
| TIMESTAMP | 8 bytes | 01.01.0001 00:00:00.0000 to 31.12.9999 23:59:59.9999 | Date and time |
| TIMESTAMP WITH TIME ZONE | 10 bytes | Timestamp with timezone | Firebird 4.0+ |

Examples:
```sql
CREATE TABLE events (
    event_date DATE,
    event_time TIME,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    scheduled_at TIMESTAMP WITH TIME ZONE    -- Firebird 4.0+
);

-- Inserting dates and times
INSERT INTO events VALUES (
    '2024-01-15',                           -- DATE
    '14:30:00',                             -- TIME
    '2024-01-15 14:30:00.0000',            -- TIMESTAMP
    '2024-01-15 14:30:00.0000 +02:00'      -- TIMESTAMP WITH TIME ZONE
);
```

## Boolean Type

| Data Type | Storage | Values | Description |
|-----------|---------|---------|-------------|
| BOOLEAN | 1 byte | TRUE, FALSE, UNKNOWN (NULL) | Logical boolean |

```sql
CREATE TABLE settings (
    setting_id INTEGER,
    is_enabled BOOLEAN DEFAULT TRUE,
    is_visible BOOLEAN DEFAULT FALSE
);

INSERT INTO settings VALUES (1, TRUE, FALSE);
INSERT INTO settings VALUES (2, 1, 0);  -- Also valid
```

## Binary Data Types

| Data Type | Storage | Description |
|-----------|---------|-------------|
| BLOB | Variable | Binary Large Object |
| BLOB SUB_TYPE 0 | Variable | Binary data (default) |
| BLOB SUB_TYPE 1 | Variable | Text data |
| BLOB SUB_TYPE < 0 | Variable | User-defined subtypes |

### BLOB Segments

```sql
CREATE TABLE documents (
    doc_id INTEGER,
    binary_data BLOB SUB_TYPE 0,           -- Binary
    text_content BLOB SUB_TYPE 1,          -- Text
    custom_data BLOB SUB_TYPE -1            -- User-defined
);

-- BLOB with segment size
CREATE TABLE images (
    image_id INTEGER,
    image_data BLOB SUB_TYPE 0 SEGMENT SIZE 4096
);
```

## Special Data Types

### ARRAY Type

Firebird supports single and multi-dimensional arrays:

```sql
CREATE TABLE matrix_data (
    matrix_id INTEGER,
    values_1d INTEGER[10],                   -- 1D array, 10 elements
    values_2d DOUBLE PRECISION[5,5],         -- 2D array, 5x5
    names VARCHAR(50)[1:10]                  -- 1D array with bounds
);

-- Inserting array data
INSERT INTO matrix_data (matrix_id, values_1d) 
VALUES (1, [1,2,3,4,5,6,7,8,9,10]);

-- Accessing array elements
SELECT values_1d[5] FROM matrix_data WHERE matrix_id = 1;
```

## Domain Types

Domains are user-defined data types based on existing types:

```sql
-- Create domains
CREATE DOMAIN D_MONEY AS NUMERIC(15,2) DEFAULT 0 
    CHECK (VALUE >= 0);

CREATE DOMAIN D_EMAIL AS VARCHAR(255) 
    CHECK (VALUE LIKE '%@%.%');

CREATE DOMAIN D_PERCENTAGE AS NUMERIC(5,2) 
    CHECK (VALUE BETWEEN 0 AND 100);

-- Use domains in tables
CREATE TABLE products (
    product_id INTEGER,
    price D_MONEY NOT NULL,
    discount D_PERCENTAGE DEFAULT 0,
    contact_email D_EMAIL
);
```

## Type Casting and Conversion

```sql
-- Explicit casting
SELECT 
    CAST('123' AS INTEGER) AS int_value,
    CAST(123.45 AS VARCHAR(10)) AS str_value,
    CAST('2024-01-15' AS DATE) AS date_value;

-- Implicit conversion
SELECT 
    '123' + 0,                    -- String to number
    123 || '',                    -- Number to string
    DATE '2024-01-15';           -- Date literal
```

## NULL Values

All data types in Firebird can store NULL unless constrained:

```sql
CREATE TABLE nullable_example (
    required_field INTEGER NOT NULL,
    optional_field INTEGER,              -- Can be NULL
    default_field INTEGER DEFAULT 0      -- Has default, can still be NULL
);
```

## Best Practices

1. **Choose appropriate numeric types**: Use INTEGER for whole numbers, NUMERIC/DECIMAL for money
2. **Size VARCHAR appropriately**: Don't use VARCHAR(8000) for short fields
3. **Use domains**: Create reusable data types with constraints
4. **Consider character sets**: Use UTF8 for international data
5. **Use BOOLEAN**: Instead of CHAR(1) or SMALLINT for true/false values
6. **Be careful with FLOAT**: Use NUMERIC/DECIMAL for financial calculations

## Data Type Selection Guide

| Use Case | Recommended Type |
|----------|------------------|
| Primary Keys | INTEGER or BIGINT |
| Money/Currency | NUMERIC(15,2) or DECIMAL(15,2) |
| Percentages | NUMERIC(5,2) |
| Names | VARCHAR(50-100) |
| Descriptions | VARCHAR(255-8000) or BLOB SUB_TYPE 1 |
| Flags | BOOLEAN |
| Timestamps | TIMESTAMP |
| Large Text | BLOB SUB_TYPE 1 |
| Binary Files | BLOB SUB_TYPE 0 |

## Next Steps

Understanding Firebird data types is crucial for:
- Creating efficient table structures
- Ensuring data integrity
- Optimizing storage and performance
- Writing portable SQL code