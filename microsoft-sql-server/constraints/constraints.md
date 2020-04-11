
Constraint is used to define rules on what values are inserted in the table. Constraint plays an important role in ensuring the accuracy and integrity of the data inserted into a table.

Commonly used constraints are as follows:
## 1. NOT NULL 
If a column is specified as NOT NULL while defining the table, then the column must not have NULL values while inserting data into the table.

### Syntax
```sql
CREATE TABLE table_name
( 
  column_name1 datatype [ NULL | NOT NULL ] [ PRIMARY KEY ],
  column_name2 datatype [ NULL | NOT NULL ],
  ...
);
```

### Example
```sql
CREATE TABLE customer
( InsuranceID INT PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  DOB DATE NOT NULL,
  NIN VARCHAR(50),
  Location VARCHAR(50)
);
```


## 2. UNIQUE
 If a column is specified as UNIQUE while defining the table, then the column must have different values for all the rows present in the table.

### Syntax
```sql
CREATE TABLE table_name
( 
  column_name1 datatype [ NULL | NOT NULL ] [ PRIMARY KEY ],
  column_name2 datatype [ NULL | NOT NULL ],
  ...
  CONSTRAINT constraint_name UNIQUE (column_name1,column_name2..)
);
```

### Example
```sql
CREATE TABLE customer
( InsuranceID INT PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  DOB DATE NOT NULL,
  NIN VARCHAR(50),
  Location VARCHAR(50),
    CONSTRAINT customer_unique UNIQUE (InsuranceID)
);
```

## 3. PRIMARY KEY 
Primary key is a column or set of columns which is used to uniquely identify a row in the database table. In the below customer table, You can either choose InsuranceID or National Insurance Number(NIN) as primary keys, but Insurance ID is preferable as NIN can be considered as personal information.

### Syntax
```sql
CREATE TABLE table_name
( 
  column_name1 datatype [ NULL | NOT NULL ] [ PRIMARY KEY ],
  column_name2 datatype [ NULL | NOT NULL ],
  ...
);
```
### Example
```sql
CREATE TABLE customer
( InsuranceID INT PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  DOB DATE NOT NULL,
  NIN VARCHAR(50),
  Location VARCHAR(50)
);
```

## 4. FOREIGN KEY 
Foreign key is a column in one table which references a primary key in other table. In the above two tables, InsuranceID of patient table can be foreign key which can be referenced to InsuranceID in customer Table.

### Syntax
```sql
CREATE TABLE table_name
(
  column_name1 datatype ,
  column_name2 datatype ,
  ...

  CONSTRAINT foreign_key_name
    FOREIGN KEY (column_name1,column_name2,..)
    REFERENCES parent_table (column_name1,column_name2,..)
    [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
    [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ] 
);
```

### Example
```sql
CREATE TABLE customer
( 
  InsuranceID INT PRIMARY KEY,  Name VARCHAR(50) NOT NULL, DOB DATE NOT NULL, NIN VARCHAR(50),
  Location VARCHAR(50)
);

CREATE TABLE patient
( HospitalID INT PRIMARY KEY, Name VARCHAR(50) NOT NULL, DOB DATE NOT NULL, InsuranceID  VARCHAR(50), CONSTRAINT fk_insurance FOREIGN KEY (InsuranceID) REFERENCES customer (InsuranceID)
);
```

## 5. CHECK 
checks for a specific condition while inserting data into the table.

### Syntax
```sql
CREATE TABLE table_name
( 
  column_name1 datatype [ NULL | NOT NULL ] [ PRIMARY KEY ],
  column_name2 datatype [ NULL | NOT NULL ],
  ...
  CONSTRAINT constraint_name
  CHECK (column_name condition)
);
```
### Example
```sql
CREATE TABLE customer
( InsuranceID INT PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  DOB DATE NOT NULL,
  NIN INT,
  Location VARCHAR(50)
  CONSTRAINT customer_chk CHECK (NIN > 0)
);
```
