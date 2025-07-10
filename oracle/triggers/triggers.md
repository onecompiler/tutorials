# Oracle Triggers

A trigger is a named PL/SQL block that executes automatically when a specific event occurs on a table or view. Triggers can respond to DML operations (INSERT, UPDATE, DELETE) and certain DDL or database events.

## Types of Triggers

### 1. DML Triggers
- **BEFORE triggers**: Execute before the triggering DML operation
- **AFTER triggers**: Execute after the triggering DML operation
- **INSTEAD OF triggers**: Replace the triggering DML operation (used with views)

### 2. Trigger Levels
- **Row-level triggers**: Execute once for each affected row (FOR EACH ROW)
- **Statement-level triggers**: Execute once for the entire SQL statement

## Basic Trigger Syntax

```sql
CREATE OR REPLACE TRIGGER trigger_name
    BEFORE | AFTER | INSTEAD OF
    INSERT | UPDATE | DELETE | UPDATE OF column_name
    ON table_name
    [REFERENCING OLD AS old NEW AS new]
    [FOR EACH ROW]
    [WHEN (condition)]
[DECLARE
    -- Variable declarations]
BEGIN
    -- Trigger body
    [EXCEPTION
        -- Exception handlers]
END trigger_name;
/
```

## Pseudo-records :NEW and :OLD

In row-level triggers, Oracle provides two pseudo-records:
- **:NEW**: Contains new values after INSERT or UPDATE
- **:OLD**: Contains old values before UPDATE or DELETE

| Operation | :NEW | :OLD |
|-----------|------|------|
| INSERT | Contains inserted values | NULL |
| UPDATE | Contains updated values | Contains original values |
| DELETE | NULL | Contains deleted values |

## Basic Trigger Examples

### 1. BEFORE INSERT Trigger
```sql
CREATE OR REPLACE TRIGGER trg_before_insert_employee
    BEFORE INSERT ON employees
    FOR EACH ROW
BEGIN
    -- Auto-generate employee ID if not provided
    IF :NEW.employee_id IS NULL THEN
        :NEW.employee_id := employees_seq.NEXTVAL;
    END IF;
    
    -- Set creation timestamp
    :NEW.created_date := SYSDATE;
    :NEW.created_by := USER;
END;
/
```

### 2. AFTER UPDATE Trigger with Audit
```sql
-- Create audit table
CREATE TABLE employees_audit (
    audit_id NUMBER PRIMARY KEY,
    employee_id NUMBER,
    action VARCHAR2(10),
    old_salary NUMBER,
    new_salary NUMBER,
    changed_by VARCHAR2(30),
    changed_date TIMESTAMP
);

CREATE OR REPLACE TRIGGER trg_employee_salary_audit
    AFTER UPDATE OF salary ON employees
    FOR EACH ROW
    WHEN (OLD.salary != NEW.salary)
BEGIN
    INSERT INTO employees_audit (
        audit_id, employee_id, action, 
        old_salary, new_salary, 
        changed_by, changed_date
    ) VALUES (
        audit_seq.NEXTVAL, :NEW.employee_id, 'UPDATE',
        :OLD.salary, :NEW.salary,
        USER, SYSTIMESTAMP
    );
END;
/
```

### 3. BEFORE DELETE Trigger with Validation
```sql
CREATE OR REPLACE TRIGGER trg_prevent_department_delete
    BEFORE DELETE ON departments
    FOR EACH ROW
DECLARE
    v_employee_count NUMBER;
BEGIN
    -- Check if department has employees
    SELECT COUNT(*)
    INTO v_employee_count
    FROM employees
    WHERE department_id = :OLD.department_id;
    
    IF v_employee_count > 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 
            'Cannot delete department with existing employees');
    END IF;
END;
/
```

## Statement-Level Triggers

### Example: Log table modifications
```sql
CREATE OR REPLACE TRIGGER trg_employees_statement_log
    AFTER INSERT OR UPDATE OR DELETE ON employees
DECLARE
    v_operation VARCHAR2(10);
BEGIN
    IF INSERTING THEN
        v_operation := 'INSERT';
    ELSIF UPDATING THEN
        v_operation := 'UPDATE';
    ELSIF DELETING THEN
        v_operation := 'DELETE';
    END IF;
    
    INSERT INTO table_activity_log (
        table_name, operation, user_name, activity_date
    ) VALUES (
        'EMPLOYEES', v_operation, USER, SYSDATE
    );
END;
/
```

## Multiple Event Triggers

```sql
CREATE OR REPLACE TRIGGER trg_employee_changes
    BEFORE INSERT OR UPDATE OR DELETE ON employees
    FOR EACH ROW
BEGIN
    IF INSERTING THEN
        :NEW.created_date := SYSDATE;
        :NEW.created_by := USER;
    ELSIF UPDATING THEN
        :NEW.modified_date := SYSDATE;
        :NEW.modified_by := USER;
        -- Preserve original creation info
        :NEW.created_date := :OLD.created_date;
        :NEW.created_by := :OLD.created_by;
    ELSIF DELETING THEN
        -- Log deletion
        INSERT INTO deleted_employees_log
        VALUES (:OLD.employee_id, :OLD.first_name, 
                :OLD.last_name, USER, SYSDATE);
    END IF;
END;
/
```

## WHEN Conditions

The WHEN clause filters when a row-level trigger fires:

```sql
CREATE OR REPLACE TRIGGER trg_high_salary_notification
    AFTER INSERT OR UPDATE OF salary ON employees
    FOR EACH ROW
    WHEN (NEW.salary > 100000)
BEGIN
    INSERT INTO high_salary_notifications (
        employee_id, salary, notification_date
    ) VALUES (
        :NEW.employee_id, :NEW.salary, SYSDATE
    );
END;
/
```

## Compound Triggers (Oracle 11g+)

Compound triggers allow you to define actions for multiple timing points in a single trigger:

```sql
CREATE OR REPLACE TRIGGER trg_employee_compound
    FOR INSERT OR UPDATE OR DELETE ON employees
    COMPOUND TRIGGER
    
    -- Global declarations
    TYPE t_emp_list IS TABLE OF employees.employee_id%TYPE;
    g_emp_list t_emp_list := t_emp_list();
    
    -- Before statement section
    BEFORE STATEMENT IS
    BEGIN
        DBMS_OUTPUT.PUT_LINE('Starting DML operation on employees');
    END BEFORE STATEMENT;
    
    -- Before each row section
    BEFORE EACH ROW IS
    BEGIN
        IF INSERTING OR UPDATING THEN
            -- Validate business rules
            IF :NEW.salary < 0 THEN
                RAISE_APPLICATION_ERROR(-20002, 'Salary cannot be negative');
            END IF;
        END IF;
    END BEFORE EACH ROW;
    
    -- After each row section
    AFTER EACH ROW IS
    BEGIN
        IF UPDATING('salary') THEN
            g_emp_list.EXTEND;
            g_emp_list(g_emp_list.COUNT) := :NEW.employee_id;
        END IF;
    END AFTER EACH ROW;
    
    -- After statement section
    AFTER STATEMENT IS
    BEGIN
        -- Process all affected employees
        FOR i IN 1..g_emp_list.COUNT LOOP
            -- Update related tables or send notifications
            UPDATE employee_statistics
            SET last_salary_update = SYSDATE
            WHERE employee_id = g_emp_list(i);
        END LOOP;
        
        DBMS_OUTPUT.PUT_LINE('Completed DML operation on employees');
    END AFTER STATEMENT;
    
END trg_employee_compound;
/
```

## INSTEAD OF Triggers for Views

```sql
-- Create a view joining multiple tables
CREATE OR REPLACE VIEW v_employee_details AS
SELECT e.employee_id, e.first_name, e.last_name,
       d.department_name, j.job_title
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN jobs j ON e.job_id = j.job_id;

-- Create INSTEAD OF trigger for the view
CREATE OR REPLACE TRIGGER trg_employee_details_update
    INSTEAD OF UPDATE ON v_employee_details
    FOR EACH ROW
BEGIN
    -- Update the base tables
    UPDATE employees
    SET first_name = :NEW.first_name,
        last_name = :NEW.last_name
    WHERE employee_id = :NEW.employee_id;
    
    -- Log the update
    INSERT INTO view_update_log (
        view_name, employee_id, update_date
    ) VALUES (
        'V_EMPLOYEE_DETAILS', :NEW.employee_id, SYSDATE
    );
END;
/
```

## Error Handling in Triggers

```sql
CREATE OR REPLACE TRIGGER trg_order_validation
    BEFORE INSERT OR UPDATE ON orders
    FOR EACH ROW
DECLARE
    v_credit_limit NUMBER;
    v_current_balance NUMBER;
    e_credit_exceeded EXCEPTION;
BEGIN
    -- Get customer credit limit and current balance
    SELECT credit_limit, current_balance
    INTO v_credit_limit, v_current_balance
    FROM customers
    WHERE customer_id = :NEW.customer_id;
    
    -- Check if order exceeds credit limit
    IF (v_current_balance + :NEW.order_total) > v_credit_limit THEN
        RAISE e_credit_exceeded;
    END IF;
    
    -- Set order date if not provided
    IF :NEW.order_date IS NULL THEN
        :NEW.order_date := SYSDATE;
    END IF;
    
EXCEPTION
    WHEN e_credit_exceeded THEN
        RAISE_APPLICATION_ERROR(-20003, 
            'Order exceeds customer credit limit');
    WHEN NO_DATA_FOUND THEN
        RAISE_APPLICATION_ERROR(-20004, 
            'Customer not found');
    WHEN OTHERS THEN
        RAISE_APPLICATION_ERROR(-20005, 
            'Error processing order: ' || SQLERRM);
END;
/
```

## Autonomous Transaction Triggers

For logging that should persist even if main transaction rolls back:

```sql
CREATE OR REPLACE TRIGGER trg_sensitive_data_access
    AFTER SELECT ON sensitive_data
    FOR EACH ROW
DECLARE
    PRAGMA AUTONOMOUS_TRANSACTION;
BEGIN
    INSERT INTO access_log (
        table_name, accessed_by, access_time, row_id
    ) VALUES (
        'SENSITIVE_DATA', USER, SYSTIMESTAMP, :NEW.id
    );
    COMMIT; -- Commits only the autonomous transaction
END;
/
```

## Managing Triggers

### View Triggers
```sql
-- View all triggers
SELECT trigger_name, trigger_type, triggering_event, 
       table_name, status
FROM user_triggers;

-- View trigger source code
SELECT text
FROM user_source
WHERE name = 'TRG_EMPLOYEE_COMPOUND'
AND type = 'TRIGGER'
ORDER BY line;
```

### Enable/Disable Triggers
```sql
-- Disable a specific trigger
ALTER TRIGGER trg_employee_audit DISABLE;

-- Enable a specific trigger
ALTER TRIGGER trg_employee_audit ENABLE;

-- Disable all triggers on a table
ALTER TABLE employees DISABLE ALL TRIGGERS;

-- Enable all triggers on a table
ALTER TABLE employees ENABLE ALL TRIGGERS;
```

### Drop Triggers
```sql
DROP TRIGGER trigger_name;
```

## Best Practices

1. **Keep triggers simple**: Complex logic should be in stored procedures
2. **Avoid recursive triggers**: Be careful with triggers that modify the same table
3. **Use naming conventions**: Prefix triggers with 'trg_' for easy identification
4. **Document trigger purpose**: Include comments explaining business logic
5. **Handle exceptions properly**: Always include error handling
6. **Test thoroughly**: Triggers can have wide-ranging effects
7. **Consider performance**: Triggers execute for every affected row
8. **Use compound triggers**: For complex multi-row operations in 11g+

## Common Use Cases

1. **Auditing**: Track changes to sensitive data
2. **Data validation**: Enforce complex business rules
3. **Automatic timestamps**: Set created/modified dates
4. **Sequence generation**: Auto-generate primary keys
5. **Referential integrity**: Custom cascade operations
6. **Data synchronization**: Keep related tables in sync
7. **Security**: Log access to sensitive data
8. **Business notifications**: Alert on specific conditions