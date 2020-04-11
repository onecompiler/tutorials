## List

List is a collection of string elements stored in order of insertion. 
* Redis Lists are implemented via Linkedin Lists.
* Insertion of element is performed in constant time regardless of size of list.
* Allows Duplicate values.

### Insertion / Pushing - LPUSH, RPUSH
* `LPUSH` key value - Inserts the value to the key at the head.
* `RPUSH` key value - Inserts the value to key at the tail.
* `LRANGE` key start stop - Displays all the elements in the key with given range.


```sh
> LPUSH whichdb redis
(integer) 1
> LRANGE whichdb 0 -1
1) "redis"

> LPUSH whichdb rabbitmq oracle
(integer) 3
> LRANGE whichdb 0 -1
1) "oracle"
2) "rabbitmq"
3) "redis"
```

```sh
> RPUSH whichdb mysql
(integer) 4
> LRANGE whichdb 0 -1
1) "oracle"
2) "rabbitmq"
3) "redis"
4) "mysql"

> RPUSH whichdb mongo elastic
(integer) 6
> LRANGE whichdb 0 -1
1) "oracle"
2) "rabbitmq"
3) "redis"
4) "mysql"
5) "mongo"
6) "elastic"
```

### Deletion / Popping - LPOP, RPOP
* `LPOP` key - retrieves and deletes the first element at the head.
* `RPOP` key - retrieve and deletes the first element at the tail.

```sh
> LPOP whichdb 
"oracle"
> LRANGE whichdb 0 -1
1) "rabbitmq"
2) "redis"
3) "mysql"
4) "mongo"
5) "elastic"

> rpop whichdb
"elastic"
> LRANGE whichdb 0 -1
1) "rabbitmq"
2) "redis"
3) "mysql"
4) "mongo"
```

### Miscellaneous 
* `LSET` key index value - Sets the list element of index to value.
* `LTRIM` key start stop -  Trims the list so that only given range of elements exist.
* `LINSERT` key BEFORE|AFTER index value - Inserts the value in the list at either after or before the index.
* `LLEN` key - Displays the length of list.

```sh
> LSET whichdb 3 postgresql
OK
> LRANGE whichdb 0 -1
1) "rabbitmq"
2) "redis"
3) "mysql"
4) "postgresql"
5) "elastic"

> LTRIM whichdb 0 1
OK
> LRANGE whichdb 0 -1
1) "rabbitmq"
2) "redis"

> LINSERT whichdb AFTER redis mysql
3
> LRANGE whichdb 0 -1
1) "rabbitmq"
2) "redis"
3) "mysql"

> LLEN whichdb
(integer) 3
```
