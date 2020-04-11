### Insert Key - SET

`SET` command is used to create key-value data .
```sh
> SET KEY VALUE
```
Key will be created if it does not exist, or else value will be updated.


```sh
> SET whichdb redis
OK
```

### Reading Key - GET, MGET

`GET` command is used to read value in key.

```sh
> GET whichdb
"redis"
```

`MGET` command is used to read values of multiple keys.
```sh
> MGET key1 key2
```

### Rename keys - RENAME

```sh
RENAME whichdb finddb
```
### Deleting Key

`DEL` command is used to delete single/multiple keys. Key is ignored if it does not exist. 

```sh
> DEL whichdb
(integer) 1
> DEL key1 key2 key3
```
### Type of Key 

```sh
> type whichdb
"string"
```


### EXPIRE Keys - EXPIRE
`EXPIRE` command is used to set timeout for a key in seconds.

```sh
> EXPIRE key 10
(integer) 1
```

`TTL` command is used to find remaining time to live for the key.

```sh
> TTL key
(integer) 10
```

Reference to all the commands in redis: https://redis.io/commands