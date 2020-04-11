## Sorted Sets

Sorted set are similar to set, a collection of unique strings, the difference is every member in a sorted set is associated with a score which is used to make sorted set ordered from smallest to greatest score.
* Redis Sorted Sets are implemented using Hash tables.
* Adding, Insertion or Updation of elements can be performed in a fast way O(logn)
* Sorted sets are used to index data stored in redis.
* It is the most advanced data type in redis.

### Insertion  - ZADD
* `ZADD` key [NX|XX] [CH] [INCR] score value - Inserts the value to the key with the given score based upon given option.
    * NX - Only Add elements if they do not exist, Ignore if they already exist.
    * XX - Only update elements if they already exist, Ignore if they  do not exist.
    * CH - Modifies the return value to number of elements changed ie.updated or added.
    * INCR - Increments the score of given element. This only works for score-element pair.
* `ZRANGE` key start stop [WITHSCORES] - Return all the elements in the sorted set with in the specified range.

```sh
> ZADD whichdb 1 "redis" 2 "rabbitmq" 3 "oracle"
(integer) 1
> ZRANGE whichdb 0 -1 WITHSCORES
1) "redis"
2) 1.0
3) "rabbitmq"
4) 2.0
5) "oracle"
6) 3.0

> ZADD NX whichdb 1 "redis" 
(integer) 0
> ZADD NX whichdb 4 "mysql" 
(integer) 1
> ZRANGE whichdb 0 -1 WITHSCORES
1) "redis"
2) 1.0
3) "rabbitmq"
4) 2.0
5) "oracle"
6) 3.0
7) "mysql"
8) 4.0
```
```sh
> ZADD whichdb XX 5 "redis"
(integer) 0
> ZRANGE whichdb 0 -1 WITHSCORES
1) "rabbitmq"
2) 2.0
3) "oracle"
4) 3.0
5) "mysql"
6) 4.0
7) "redis"
8) 5.0

> ZADD whichdb CH 1 "redis"  5 "mysql"
(integer) 2
> ZRANGE whichdb 0 -1 WITHSCORES
1) "redis"
2) 1.0
3) "rabbitmq"
4) 2.0
5) "oracle"
6) 3.0
7) "mysql"
8) 5.0

> ZADD whichdb INCR 1 "redis"
(integer) 2
> ZRANGE whichdb 0 -1 WITHSCORES
1) "rabbitmq"
2) 2.0
3) "redis"
4) 2.0
5) "oracle"
6) 3.0
7) "mysql"
8) 5.0
```

### Deletion - ZREM, ZREMRANGEBYRANK, ZREMRANGEBYSCORE
* `ZREM` key member - Removes the specified members from the sorted set for given key.
* `ZREMRANGEBYRANK` key start stop - Removes all the elements with in specified rank from the key.
* `ZREMRANGEBYSCORE` key start stop - Removes all the elements with in specified score from the key.

```sh
> ZREM whichdb "mysql"
1
> ZRANGE whichdb 0 -1 WITHSCORES
1) "rabbitmq"
2) 2.0
3) "redis"
4) 2.0
5) "oracle"
6) 3.0

> ZREMRANGEBYRANK whichdb 1 2
(integer) 2
> ZRANGE whichdb 0 -1 WITHSCORES
1) "rabbitmq"
2) 2.0

> ZREMRANGEBYRANK whichdb 1 2
(integer) 1
> ZRANGE whichdb 0 -1 WITHSCORES
(empty list or set)
```

### Miscellaneous 
* `ZCARD` key - Returns the number of elements stored in a sorted set.
* `ZCOUNT` key min max-  Returns the number(count) of elements in sorted set with scores in given range.
* `ZRANK` key member- Returns the rank or index of a member in a sorted set.
* `ZSCORE` key member- Returns the score of a member in a sorted set.

```sh
> ZRANGE sports 0 -1 withscores
1) "cricket"
2) 1.0
3) "baseball"
4) 3.0
5) "football"
6) 6.0
7) "hockey"
8) 6.0
9) "basketball"
10) 9.0

> ZCARD sports
5

> ZCOUNT sports 3 7
3

> ZRANK sports "hockey"
3

> ZSCORE sports "basketball"
9.0
```
