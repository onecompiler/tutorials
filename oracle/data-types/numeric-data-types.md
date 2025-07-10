# Oracle Numeric Data Types

## 1. NUMBER(p,s)
The NUMBER data type is Oracle's primary numeric data type. It stores both fixed and floating-point numbers.

- **p (precision)**: Total number of digits (1 to 38)
- **s (scale)**: Number of digits to the right of decimal point (-84 to 127)

### Examples:
```sql
-- Integer
NUMBER(10)      -- Integer with up to 10 digits
NUMBER(10,0)    -- Same as NUMBER(10)

-- Fixed-point decimal
NUMBER(10,2)    -- Up to 8 digits before decimal, 2 after (e.g., 12345678.99)

-- Floating-point
NUMBER          -- No precision/scale specified, stores any numeric value

-- Specific examples
CREATE TABLE products (
    product_id NUMBER(10),
    price NUMBER(10,2),
    quantity NUMBER(5),
    tax_rate NUMBER(3,2)  -- e.g., 0.15 for 15%
);
```

### Common NUMBER Subtypes:
- **INTEGER**: Equivalent to NUMBER(38)
- **SMALLINT**: Equivalent to NUMBER(38)
- **DECIMAL(p,s)**: ANSI-compatible, equivalent to NUMBER(p,s)
- **NUMERIC(p,s)**: ANSI-compatible, equivalent to NUMBER(p,s)
- **DEC(p,s)**: Equivalent to DECIMAL(p,s)

## 2. BINARY_FLOAT
32-bit single-precision floating-point number (IEEE 754 format).

- **Range**: 1.17549E-38F to 3.40282E+38F
- **Storage**: 4 bytes
- **Special values**: BINARY_FLOAT_NAN, BINARY_FLOAT_INFINITY, -BINARY_FLOAT_INFINITY

### Example:
```sql
CREATE TABLE scientific_data (
    measurement_id NUMBER(10),
    temperature BINARY_FLOAT,
    pressure BINARY_FLOAT
);

-- Inserting values
INSERT INTO scientific_data VALUES (1, 98.6F, 1013.25F);
INSERT INTO scientific_data VALUES (2, BINARY_FLOAT_INFINITY, 0F);
```

## 3. BINARY_DOUBLE
64-bit double-precision floating-point number (IEEE 754 format).

- **Range**: 2.22507485850720E-308 to 1.79769313486231E+308
- **Storage**: 8 bytes
- **Special values**: BINARY_DOUBLE_NAN, BINARY_DOUBLE_INFINITY, -BINARY_DOUBLE_INFINITY

### Example:
```sql
CREATE TABLE precise_calculations (
    calc_id NUMBER(10),
    result BINARY_DOUBLE,
    error_margin BINARY_DOUBLE
);

-- Inserting values
INSERT INTO precise_calculations VALUES (1, 3.141592653589793D, 0.000000000000001D);
```

## Performance Considerations

1. **NUMBER**: Most flexible but slower for complex calculations
2. **BINARY_FLOAT/BINARY_DOUBLE**: Faster for scientific calculations but may have rounding errors
3. Use NUMBER for financial data where precision is critical
4. Use BINARY_* types for scientific/engineering applications where performance matters

## Numeric Functions

Oracle provides numerous functions for numeric data types:
```sql
-- Rounding functions
SELECT ROUND(123.456, 2) FROM dual;      -- 123.46
SELECT TRUNC(123.456, 2) FROM dual;      -- 123.45
SELECT CEIL(123.456) FROM dual;          -- 124
SELECT FLOOR(123.456) FROM dual;         -- 123

-- Mathematical functions
SELECT POWER(2, 10) FROM dual;           -- 1024
SELECT SQRT(16) FROM dual;               -- 4
SELECT MOD(10, 3) FROM dual;             -- 1
```


