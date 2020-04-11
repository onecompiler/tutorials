## String

It is basic data type which can store Strings, Integer and floating points

```sh
> SET rank 1
OK
> SET average 4.5
OK
> SET name redis
OK
```

Redis provides basic commands to manage strings

### Increment - INCR, INCRBY, INCRBYFLOAT

INCR, INCRBY and INCRBYFLOAT are used to increase the value of integer or float value stored in a string

* `INCR` key- Increments the value of integer by 1
* `INCRBY` key- Increments the value of integer by given amount
* `INCRBYFLOAT` key value- Increments the floating value by given amount ( Applicable for integers also)
```sh
> INCR rank
(integer) 2
> GET rank
"2"

> INCRBY rank 3
(integer) 5
> GET rank
"5"

> INCRBYFLOAT average 2.3
6.8
> GET average
"6.8"
```

### Decrement - DECR, DECRBY

DECR, DECRBY are used to decrease the value of integer  value stored in a string

* `DECR` key- Decrements the value of integer by 1
* `DECRBY` key value- Decrements the value of integer by given amount

```sh
> GET rank
"5"
> DECR rank
(integer) 4
> GET rank
"4"

> DECRBY rank 3
(integer) 1
> GET rank
"1"
```

### String Manipulation - SETRANGE, APPEND, STRLEN

SETRANGE, APPEND, SETRANGE are used manipulation of value stored in a string

* `SETRANGE` key offest value- Overwrites the part of string at the given offset with given value
* `APPEND` key value- Appends the value to the string
* `STRLEN` key- Determines the length of string

```sh
> GET name
"redis"
> SETRANGE name 1 ichard
7
> GET name
"richard"

> APPEND name son
10
> GET name
"richardson"

> STRLEN name
(integer) 10
```