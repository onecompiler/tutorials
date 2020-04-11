## Hashes

Hashes is a collection of maps between string fields and string values.
* Hashes are mainly used to represent objects.
* Addition and Retreiving the elements are performed in constant time.

### Insertion  - HMSET, HSET
* `HMSET` key fields1 value1 field2 value2 - Used to set multiple hash fields to multiple values.
* `HGETALL` key - Returns all the fields and values from the given key.
* `HSET` key field value - Used to set given hash field to value.
* `HGET` key field- Returns the value of given hash field in key.


```sh
> HMSET redis name "redis" type "Message Broker" website "https://redis.io/" 
(integer) 3
> HGETALL redis
1) "name"
2) "redis"
3) "type"
4) "Message Broker"
5) "website"
6) "https://redis.io/"

> HSET redis opensource "true"
(integer) 1
> HGET redis opensource
"true"
```
### Deletion - HDEL
* `HDEL` key field1 field2 - Removes the specified fields from given key.

```sh
> HDEL redis opensource
(integer) 1
> HGETALL redis
1) "name"
2) "redis"
3) "type"
4) "Message Broker"
5) "website"
6) "https://redis.io/"
```

### Miscellaneous 
* `HKEYS` key - Returns all the fields stored in a key.
* `HVALS` key -  Retunr all the values in key.
* `HLEN` key - Returns the number of fields in key.

```sh
> HKEYS redis
1) "name"
2) "type"
3) "website"

> HVALS redis
1) "redis"
2) "Message Broker"
3) "https://redis.io/"

> HLEN redis
3
```
