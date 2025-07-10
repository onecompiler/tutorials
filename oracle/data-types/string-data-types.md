# Oracle String Data Types

## 1. VARCHAR2(size [BYTE | CHAR])
Variable-length character string. This is Oracle's recommended string data type.

- **Maximum size**: 4000 bytes (standard), 32767 bytes (with MAX_STRING_SIZE=EXTENDED)
- **Storage**: Only stores actual data length + 2 bytes for length information
- **Character semantics**: Can specify BYTE (default) or CHAR

### Examples:
```sql
-- Byte semantics (default)
VARCHAR2(100)       -- Up to 100 bytes
VARCHAR2(100 BYTE)  -- Explicitly byte semantics

-- Character semantics
VARCHAR2(100 CHAR)  -- Up to 100 characters (multi-byte safe)

-- Table example
CREATE TABLE employees (
    emp_id NUMBER(10),
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    email VARCHAR2(100),
    comments VARCHAR2(4000)  -- Maximum standard size
);
```

## 2. CHAR(size [BYTE | CHAR])
Fixed-length character string. Always padded with spaces to the specified length.

- **Maximum size**: 2000 bytes
- **Storage**: Always uses full specified length
- **Use cases**: Fixed-length codes, flags

### Examples:
```sql
-- Fixed length fields
CHAR(10)        -- Always 10 bytes, space-padded
CHAR(2)         -- For country codes, state codes

-- Table example
CREATE TABLE countries (
    country_code CHAR(2),       -- Always 2 characters
    iso3_code CHAR(3),          -- Always 3 characters
    phone_code VARCHAR2(5),     -- Variable length
    country_name VARCHAR2(100)
);

-- Comparison behavior
'US' stored in CHAR(5) becomes 'US   ' (with 3 spaces)
```

## 3. CLOB (Character Large Object)
Large character data storage.

- **Maximum size**: 4 GB - 1 (4,294,967,295 characters)
- **Storage**: Out-of-line storage for large data
- **Use cases**: Documents, XML, JSON, long text

### Examples:
```sql
CREATE TABLE documents (
    doc_id NUMBER(10),
    title VARCHAR2(200),
    content CLOB,
    created_date DATE
);

-- Inserting CLOB data
INSERT INTO documents VALUES (
    1,
    'User Manual',
    'This is a very long document content...',
    SYSDATE
);

-- Working with CLOBs
DECLARE
    v_clob CLOB;
BEGIN
    -- Initialize empty CLOB
    DBMS_LOB.CREATETEMPORARY(v_clob, TRUE);
    
    -- Append data
    DBMS_LOB.APPEND(v_clob, 'First part of text');
    DBMS_LOB.APPEND(v_clob, 'Second part of text');
END;
```

## 4. NVARCHAR2(size)
Variable-length Unicode character string.

- **Maximum size**: 4000 bytes (standard), 32767 bytes (extended)
- **Character set**: AL16UTF16 or UTF8 (national character set)
- **Use cases**: Multilingual data

### Examples:
```sql
CREATE TABLE international_products (
    product_id NUMBER(10),
    name_english VARCHAR2(100),
    name_local NVARCHAR2(100),  -- Stores Unicode characters
    description NVARCHAR2(1000)
);

-- Inserting multilingual data
INSERT INTO international_products VALUES (
    1,
    'Green Tea',
    N'绿茶',  -- Chinese characters
    N'传统中国绿茶'
);
```

## 5. NCHAR(size)
Fixed-length Unicode character string.

- **Maximum size**: 2000 bytes
- **Character set**: National character set
- **Behavior**: Same as CHAR but for Unicode

### Examples:
```sql
CREATE TABLE currency_symbols (
    currency_code CHAR(3),
    symbol NCHAR(2),  -- Unicode currency symbols
    country VARCHAR2(50)
);

INSERT INTO currency_symbols VALUES ('EUR', N'€', 'European Union');
INSERT INTO currency_symbols VALUES ('JPY', N'¥', 'Japan');
```

## 6. NCLOB (National Character Large Object)
Large Unicode character data storage.

- **Maximum size**: 4 GB - 1
- **Character set**: National character set
- **Use cases**: Large multilingual documents

### Examples:
```sql
CREATE TABLE multilingual_docs (
    doc_id NUMBER(10),
    language VARCHAR2(50),
    content NCLOB
);
```

## String Functions

Oracle provides extensive string manipulation functions:
```sql
-- Length functions
SELECT LENGTH('Hello') FROM dual;           -- 5 (character count)
SELECT LENGTHB('Hello') FROM dual;          -- 5 (byte count)

-- Case conversion
SELECT UPPER('hello'), LOWER('HELLO'), INITCAP('hello world') FROM dual;

-- Trimming
SELECT TRIM('  hello  ') FROM dual;         -- 'hello'
SELECT LTRIM('  hello'), RTRIM('hello  ') FROM dual;

-- Substring
SELECT SUBSTR('Hello World', 1, 5) FROM dual;  -- 'Hello'

-- Concatenation
SELECT 'Hello' || ' ' || 'World' FROM dual;    -- 'Hello World'
SELECT CONCAT('Hello', 'World') FROM dual;     -- 'HelloWorld'
```

## Best Practices

1. **Use VARCHAR2** instead of VARCHAR (VARCHAR may change in future)
2. **Use VARCHAR2** over CHAR unless you need fixed-length data
3. **Consider character semantics** for international applications
4. **Use CLOB** for data larger than 4000 bytes
5. **Use N-prefixed types** (NVARCHAR2, NCHAR, NCLOB) for Unicode data