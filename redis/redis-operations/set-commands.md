## Sets

Set is a collection of unordered, non-repeating string values.
* Redis Sets are implemented using Hash tables.
* Adding, Insertion and checking existence of value is performed in constant time irrespective of size.
* Redis sets support union, intersection and differences of sets.
* Can store a maximum of 4 billion elements for a set.

### Insertion  - SADD
* `SADD` key value1 value2 - Inserts the values to the key.
* `SMEMBERS` key - Return all the elements in the set.

```sh
> SADD odd 5 3 3 1
(integer) 3
> SMEMBERS odd
1) "1"
2) "3"
3) "5"

> SADD prime 2 3 5 5 7
(integer) 4
> SMEMBERS prime
1) "2"
2) "3"
3) "5"
4) "7"
```
### Deletion - SREM, SPOP
* `SREM` key1 value1 value2 - Removes the specified members from the set for given key.
* `SPOP` key - Removes and returns one or more members from the key.

```sh
> SREM prime 5
1
> SMEMBERS prime
1) "2"
2) "3"
3) "7"

> SPOP prime
"3"
> SMEMBERS prime
1) "2"
2) "7"
```

### Set Operations - SINTER, SUNION, SDIFF
* `SINTER` key1 key2 - Returns the members of the set resulting from intersection of all given sets.
* `SUNION` key1 key2 - Returns the members of the set resulting from union of all given sets.
* `SDIFF` key1 key2 - Returns the members of the set resulting from difference of all given sets.

```sh
> SINTER odd prime 
1) "3"
2) "5"

> SUNION odd prime
1) "2"
2) "3"
3) "5"
4) "7"

> SDIFF odd prime
1) "2"
2) "7"
```

### Miscellaneous 
* `SCARD` key - Returns the number of elements stored in a key.
* `SRANDMEMBER` key count-  Retunr the given number(count) of random elements in key.
* `SISMEMBER` key value- Returns if a given element already exist in the provided set or not.

```sh
> SMEMBERS odd
1) "1"
2) "3"
3) "5"
4) "7"

> SCARD odd
4

> SRANDMEMBER odd 2
1) "7"
2) "2"

> SISMEMBER odd 3
(integer) 1

> SISMEMBER odd 4
(integer) 0
```
