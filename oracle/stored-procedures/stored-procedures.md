# Oracle Stored Procedures

A stored procedure in Oracle is a named PL/SQL block that performs a specific task. It's compiled and stored in the database and can be invoked by multiple applications. Stored procedures can accept parameters, perform complex logic, and handle exceptions.

## Basic Syntax

```sql
CREATE OR REPLACE PROCEDURE procedure_name 
    (parameter1 [IN | OUT | IN OUT] datatype,
     parameter2 [IN | OUT | IN OUT] datatype, ...)
IS
    -- Variable declarations
BEGIN
    -- Procedure logic
EXCEPTION
    -- Exception handling
END procedure_name;
/
```

## Parameter Modes

- **IN**: Input parameter (default). Value is passed to the procedure and cannot be changed inside.
- **OUT**: Output parameter. Used to return values from the procedure.
- **IN OUT**: Bidirectional parameter. Can accept input and return output.

## Simple Procedure Example

```sql
CREATE OR REPLACE PROCEDURE greet_user (
    p_name IN VARCHAR2
)
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello, ' || p_name || '!');
END greet_user;
/
```

### Execute the procedure:
```sql
-- Enable output display
SET SERVEROUTPUT ON;

-- Call the procedure
BEGIN
    greet_user('John');
END;
/

-- Or use EXECUTE
EXECUTE greet_user('John');
```

## Procedure with OUT Parameter

```sql
CREATE OR REPLACE PROCEDURE calculate_tax (
    p_salary IN NUMBER,
    p_tax OUT NUMBER
)
IS
BEGIN
    IF p_salary <= 50000 THEN
        p_tax := p_salary * 0.1;  -- 10% tax
    ELSIF p_salary <= 100000 THEN
        p_tax := p_salary * 0.2;  -- 20% tax
    ELSE
        p_tax := p_salary * 0.3;  -- 30% tax
    END IF;
END calculate_tax;
/
```

### Execute with OUT parameter:
```sql
DECLARE
    v_salary NUMBER := 75000;
    v_tax NUMBER;
BEGIN
    calculate_tax(v_salary, v_tax);
    DBMS_OUTPUT.PUT_LINE('Tax for salary ' || v_salary || ' is: ' || v_tax);
END;
/
```

## Procedure with IN OUT Parameter

```sql
CREATE OR REPLACE PROCEDURE format_phone_number (
    p_phone IN OUT VARCHAR2
)
IS
BEGIN
    -- Remove all non-numeric characters
    p_phone := REGEXP_REPLACE(p_phone, '[^0-9]', '');
    
    -- Format as (XXX) XXX-XXXX
    IF LENGTH(p_phone) = 10 THEN
        p_phone := '(' || SUBSTR(p_phone, 1, 3) || ') ' || 
                   SUBSTR(p_phone, 4, 3) || '-' || 
                   SUBSTR(p_phone, 7, 4);
    END IF;
END format_phone_number;
/
```

### Execute with IN OUT parameter:
```sql
DECLARE
    v_phone VARCHAR2(20) := '123-456-7890';
BEGIN
    DBMS_OUTPUT.PUT_LINE('Before: ' || v_phone);
    format_phone_number(v_phone);
    DBMS_OUTPUT.PUT_LINE('After: ' || v_phone);
END;
/
```

## Procedure with Multiple Parameters and Local Variables

```sql
CREATE OR REPLACE PROCEDURE order_summary (
    p_customer_id IN VARCHAR2,
    p_start_date IN DATE,
    p_end_date IN DATE,
    p_total_amount OUT NUMBER,
    p_order_count OUT NUMBER
)
IS
    -- Local variables
    v_avg_amount NUMBER;
    v_customer_name VARCHAR2(100);
BEGIN
    -- Get customer name
    SELECT customer_name INTO v_customer_name
    FROM customers
    WHERE customer_id = p_customer_id;
    
    -- Calculate total amount and count
    SELECT COUNT(*), NVL(SUM(order_amount), 0)
    INTO p_order_count, p_total_amount
    FROM orders
    WHERE customer_id = p_customer_id
    AND order_date BETWEEN p_start_date AND p_end_date;
    
    -- Calculate average
    IF p_order_count > 0 THEN
        v_avg_amount := p_total_amount / p_order_count;
        DBMS_OUTPUT.PUT_LINE('Customer: ' || v_customer_name);
        DBMS_OUTPUT.PUT_LINE('Average Order Amount: ' || v_avg_amount);
    END IF;
    
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Customer ID ' || p_customer_id || ' not found');
        p_total_amount := 0;
        p_order_count := 0;
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
        RAISE;
END order_summary;
/
```

## Procedure with Exception Handling

```sql
CREATE OR REPLACE PROCEDURE transfer_funds (
    p_from_account IN NUMBER,
    p_to_account IN NUMBER,
    p_amount IN NUMBER,
    p_success OUT VARCHAR2
)
IS
    v_balance NUMBER;
    e_insufficient_funds EXCEPTION;
    e_invalid_amount EXCEPTION;
BEGIN
    -- Validate amount
    IF p_amount <= 0 THEN
        RAISE e_invalid_amount;
    END IF;
    
    -- Check balance
    SELECT balance INTO v_balance
    FROM accounts
    WHERE account_id = p_from_account
    FOR UPDATE; -- Lock the row
    
    IF v_balance < p_amount THEN
        RAISE e_insufficient_funds;
    END IF;
    
    -- Perform transfer
    UPDATE accounts
    SET balance = balance - p_amount
    WHERE account_id = p_from_account;
    
    UPDATE accounts
    SET balance = balance + p_amount
    WHERE account_id = p_to_account;
    
    COMMIT;
    p_success := 'Transfer successful';
    
EXCEPTION
    WHEN e_insufficient_funds THEN
        ROLLBACK;
        p_success := 'Error: Insufficient funds';
    WHEN e_invalid_amount THEN
        ROLLBACK;
        p_success := 'Error: Invalid amount';
    WHEN NO_DATA_FOUND THEN
        ROLLBACK;
        p_success := 'Error: Account not found';
    WHEN OTHERS THEN
        ROLLBACK;
        p_success := 'Error: ' || SQLERRM;
END transfer_funds;
/
```

## Procedure with Cursor

```sql
CREATE OR REPLACE PROCEDURE generate_employee_report (
    p_department_id IN NUMBER
)
IS
    -- Cursor declaration
    CURSOR c_employees IS
        SELECT employee_id, first_name, last_name, salary
        FROM employees
        WHERE department_id = p_department_id
        ORDER BY salary DESC;
    
    -- Variables
    v_total_salary NUMBER := 0;
    v_emp_count NUMBER := 0;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee Report for Department: ' || p_department_id);
    DBMS_OUTPUT.PUT_LINE('=====================================');
    
    -- Process cursor
    FOR emp_rec IN c_employees LOOP
        v_emp_count := v_emp_count + 1;
        v_total_salary := v_total_salary + emp_rec.salary;
        
        DBMS_OUTPUT.PUT_LINE(
            RPAD(emp_rec.employee_id, 10) || 
            RPAD(emp_rec.first_name || ' ' || emp_rec.last_name, 30) ||
            TO_CHAR(emp_rec.salary, '$999,999.99')
        );
    END LOOP;
    
    DBMS_OUTPUT.PUT_LINE('=====================================');
    DBMS_OUTPUT.PUT_LINE('Total Employees: ' || v_emp_count);
    DBMS_OUTPUT.PUT_LINE('Total Salary: ' || TO_CHAR(v_total_salary, '$999,999.99'));
    
    IF v_emp_count > 0 THEN
        DBMS_OUTPUT.PUT_LINE('Average Salary: ' || 
            TO_CHAR(v_total_salary / v_emp_count, '$999,999.99'));
    END IF;
END generate_employee_report;
/
```

## Managing Stored Procedures

### View procedure source code:
```sql
SELECT text
FROM user_source
WHERE name = 'PROCEDURE_NAME'
AND type = 'PROCEDURE'
ORDER BY line;
```

### View all procedures:
```sql
SELECT object_name, status
FROM user_objects
WHERE object_type = 'PROCEDURE'
ORDER BY object_name;
```

### Drop a procedure:
```sql
DROP PROCEDURE procedure_name;
```

### Recompile a procedure:
```sql
ALTER PROCEDURE procedure_name COMPILE;
```

## Best Practices

1. **Use meaningful names**: Name procedures clearly to indicate their purpose
2. **Parameter naming**: Prefix parameters (p_ for parameters, v_ for variables)
3. **Error handling**: Always include exception handling
4. **Documentation**: Add comments to explain complex logic
5. **Transaction control**: Use COMMIT/ROLLBACK appropriately
6. **Avoid DDL**: Don't use DDL statements in procedures (they cause implicit commits)

## Example: Complete Order Processing Procedure

```sql
CREATE OR REPLACE PROCEDURE process_order (
    p_customer_id IN NUMBER,
    p_items IN VARCHAR2,  -- Comma-separated item IDs
    p_order_id OUT NUMBER,
    p_status OUT VARCHAR2
)
IS
    -- Constants
    c_tax_rate CONSTANT NUMBER := 0.08;
    
    -- Variables
    v_item_id NUMBER;
    v_item_price NUMBER;
    v_quantity NUMBER;
    v_subtotal NUMBER := 0;
    v_tax NUMBER;
    v_total NUMBER;
    
    -- Exception
    e_invalid_customer EXCEPTION;
    PRAGMA EXCEPTION_INIT(e_invalid_customer, -20001);
BEGIN
    -- Validate customer
    DECLARE
        v_count NUMBER;
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM customers
        WHERE customer_id = p_customer_id
        AND status = 'ACTIVE';
        
        IF v_count = 0 THEN
            RAISE_APPLICATION_ERROR(-20001, 'Invalid or inactive customer');
        END IF;
    END;
    
    -- Create order header
    INSERT INTO orders (customer_id, order_date, status)
    VALUES (p_customer_id, SYSDATE, 'PENDING')
    RETURNING order_id INTO p_order_id;
    
    -- Process items (simplified parsing)
    -- In real scenario, use a collection or temporary table
    
    -- Calculate totals
    v_tax := v_subtotal * c_tax_rate;
    v_total := v_subtotal + v_tax;
    
    -- Update order totals
    UPDATE orders
    SET subtotal = v_subtotal,
        tax = v_tax,
        total = v_total,
        status = 'CONFIRMED'
    WHERE order_id = p_order_id;
    
    COMMIT;
    p_status := 'Order processed successfully';
    
EXCEPTION
    WHEN e_invalid_customer THEN
        ROLLBACK;
        p_status := 'Error: Invalid customer';
        p_order_id := NULL;
    WHEN OTHERS THEN
        ROLLBACK;
        p_status := 'Error: ' || SQLERRM;
        p_order_id := NULL;
END process_order;
/
```

## Debugging Stored Procedures

Enable DBMS_OUTPUT for debugging:
```sql
SET SERVEROUTPUT ON SIZE UNLIMITED;
```

Add debug statements in your procedure:
```sql
DBMS_OUTPUT.PUT_LINE('Debug: Variable value = ' || v_variable);
```

## Performance Considerations

1. Use bind variables instead of concatenating values
2. Minimize context switches between SQL and PL/SQL
3. Use bulk operations (BULK COLLECT, FORALL) for large datasets
4. Consider using pipelined functions for large result sets
5. Compile procedures with optimization level:
   ```sql
   ALTER PROCEDURE procedure_name COMPILE PLSQL_OPTIMIZE_LEVEL=2;
   ```