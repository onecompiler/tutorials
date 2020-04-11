
Trigger is a set of actions which gets executed automatically when a specific event occurs like INSERT, UPDATE or DELETE.

## How to create a TRIGGER?

#### Syntax:
```sql
CREATE TRIGGER trigger_name ON tbl_name
{FOR|AFTER|INSTEAD OF} { [ INSERT ] [ , ] [ UPDATE ] [ , ] [ DELETE ] } AS
SQL Statements;
```
#### Example:
In the below example, we will see ORDER_HISTORY table automaticlaly gets updated after a new record is inserted into ORDERS table. This is an example of AFTER INSERT, similarly we can use triggers for INSERT/UPDATE/DELETE events and based on the requirement choose whether the update on the second table should happen BEFORE or AFTER.

```sql
CREATE TABLE ORDERS(ORDERID INT,CUSTOMERID VARCHAR(50),ITEM VARCHAR(50),BILL_AMOUNT DECIMAL(10,2));
CREATE TABLE ORDER_HISTORY(ORDERID VARCHAR(50),ITEMS VARCHAR(50),BILL_AMOUNT DECIMAL(10,2),DATE_OF_ORDER DATE);

--creating trigger
CREATE TRIGGER ORD_HIS ON ORDERS AFTER INSERT AS
INSERT INTO ORDER_HISTORY values( new.ORDERID,new.ITEM,new.BILL_AMOUNT,NOW());

--Now insert values into ORDERS table:
INSERT INTO ORDERS VALUES(123,'C10','MANGO',5);
```
Check the ORDER_HISTORY table which will yield below results:

|ORDERID|ITEM|WALLETAMOUNT|DATE_OF_ORDER|
|--- |---|---|---|
|123|Mango|5.00|2019-10-01|

## How to delete a TRIGGER?

#### Syntax:
```sql
 DROP TRIGGER trigger_name;
```

In the above example, if we want to delete the trigger we created,
```sql
DROP TRIGGER  ORD_HIS;
```
