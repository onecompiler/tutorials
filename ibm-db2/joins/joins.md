
Joins are used along with SELECT statement whenever there is a need to retrieve data from multiple tables.

Commonly used Joins in DB2 are as follows:

1. INNER JOIN
2. LEFT JOIN (LEFT OUTER JOIN)
3. RIGHT JOIN (RIGHT OUTER JOIN)
4. CROSS JOIN
5. FULL OUTER JOIN

Let us consider the below tables for our understanding:


TABLE1: 


|A|Z|
|---|---|
|a|1|
|b|4|
|d|5|


TABLE2:                    

|A|X|
|---|---|
|a|10|
|e|17|
|d|30|

## 1. INNER JOIN
INNER JOIN combines the values from both the tables based on matching criteria.

Below is a basic example of how inner join works:
```sql
SELECT * FROM TABLE1 INNER JOIN TABLE2 where TABLE1.A=TABLE2.A;
```

#### Result:

|A|Z|A|X|
|--|--|--|--|
|a|1|a|10|
|d|5|d|30|

## 2. LEFT JOIN
LEFT JOIN returns all the values from the Left hand table of the condition and returns only the matching values from the second table. The unmatched row values of right hand table will be represented as NULL.
```sql
SELECT * FROM TABLE1 LEFT JOIN TABLE2 ON TABLE1.A=TABLE2.A;
```
#### Result:

|A|Z|A|X|
|---|---|---|---|
|a|1|a|10|
|d|5|d|30|
|b|4|null|null|

## 3. RIGHT JOIN
RIGHT JOIN returns all the values from the right hand table of the condition and returns only the matching values from the left hand table. The unmatched row values of left hand table will be represented as NULL.
```sql
SELECT * FROM TABLE1 RIGHT JOIN TABLE2 ON TABLE1.A=TABLE2.A;
```
#### Result:

|A|Z|A|X|
|---|---|---|---|
|a|1|a|10|
|d|5|d|30|
|null|null|e|17|

## 4. CROSS JOIN
CROSS JOIN matches each row of the first table with every row of the second table.
```sql
SELECT A,Z,X from TABLE1 CROSS JOIN TABLE2;
```
#### Result:

|A|Z|X|
|---|---|---|
|a|1|10|
|a|1|17|
|a|1|30|
|b|4|10|
|b|4|17|
|b|4|30|
|d|5|10|
|d|5|17|
|d|5|30|

## 5. FULL OUTER JOIN
FULL OUTER JOIN returns all records when there is a match in either left or right table. This combines the results of both LEFT and RIGHT joins. Records from both tables that don't have matching values in the other table will have NULL values for the columns from the other table.

```sql
SELECT * FROM TABLE1 FULL OUTER JOIN TABLE2 ON TABLE1.A=TABLE2.A;
```
#### Result:

|A|Z|A|X|
|---|---|---|---|
|a|1|a|10|
|d|5|d|30|
|b|4|null|null|
|null|null|e|17|

## DB2-Specific Join Features

### 1. Join with FETCH FIRST clause
DB2 allows combining joins with the FETCH FIRST clause to limit results:
```sql
SELECT * FROM TABLE1 
INNER JOIN TABLE2 ON TABLE1.A=TABLE2.A
FETCH FIRST 5 ROWS ONLY;
```

### 2. Lateral Joins
DB2 supports LATERAL joins which allow a derived table in the FROM clause to reference columns from preceding tables:
```sql
SELECT T1.*, T2.*
FROM TABLE1 T1,
LATERAL (SELECT * FROM TABLE2 WHERE TABLE2.A = T1.A) T2;
```

### 3. Join with Table Functions
DB2 allows joining with table functions:
```sql
SELECT T1.*, TF.*
FROM TABLE1 T1
INNER JOIN TABLE(table_function(T1.A)) AS TF ON 1=1;
```
