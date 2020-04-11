CQL also provides collection data types. Following list provides different data types available in cassandra.

1. List - Collection of one or more ordered elements.
1. map - Collection of key-value pairs.
1. set - Collection of one or more elments.

### List
List is used to maintain the order of elements allowing duplicates.

* Creating a Table with List
```sh
CREATE TABLE employee(id int PRIMARY KEY, name text, email list<text>);
```

* Inserting Data into List
```sh
INSERT INTO employee(id, name, email) VALUES (1, 'john',
['john@gmail.com','john123ba@yahoo.com'])
```
* Updating a List
```sh
UPDATE employee SET email = email +['johnabc@live.com']
WHERE name = 'john';
```

### SET
SET is used to store group of data and the elements will be returned in sorted order.

* Creating a Table with Set
```sh
CREATE TABLE employee2(id int PRIMARY KEY, name text, mobile set<varint>);
```

* Inserting Data into Set
```sh
INSERT INTO employee2(id, name, mobile) VALUES (1, 'kevin',
{9999911122, 9998855664})
```
* Updating a List
```sh
UPDATE employee2 SET mobile = mobile +{9898989898}
WHERE name = 'kevin';
```


### MAP
Map is used to store key-value pair of elements.

* Creating a Table with Map
```sh
CREATE TABLE employee3(id int PRIMARY KEY, name text, address map<text, text>);
```

* Inserting Data into Map
```sh
INSERT INTO employee3(id, name, address) VALUES (1, 'smith',
{'home':'san jose','work':'chicago'})
```
* Updating a Map
```sh
UPDATE employee3 SET address = address +{'permanent':'New york'}
WHERE name = 'smith';
```
