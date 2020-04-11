### Transactions

Redis transactions allow execution of multiple commands in a single step.
*  All the commands are serialized and executed in a sequential manner.
*  Redis transactions are atomic, either all the commands or none are processed.
*  Transaction is a single isolate operation, No other request is served in the middle of transaction.
*  MULTI, EXEC, DISCARD AND WATCH are commands used in a transaction.

## MULTI
*  `MULTI` command is used to initiate a redis transaction .
*  At this point user can issue mulitple commands.  
*  Instead of executing all the commands, redis will keep them in queue.


## EXEC
* Executes all previously queued commands in a transaction and restores the state to normal.

```sh
> MULTI
OK
> INCR alpha
QUEUED
> INCR beta
QUEUED
> EXEC
1) (integer) 1
2) (integer) 1
```

* Returns an array of replies, in which every element is reply of each command in the same order.

## DISCARD
* `DISCARD` is used to abort the transaction.
* State of the connection is restored to normal.

```sh
> MULTI
OK
> INCR alpha
QUEUED
> DISCARD
OK
> GET alpha
"1"
```

## WATCH
* `WATCH` Marks the given key to be watched for conditional exection of transaction.
* Transaction will be only be performed if none of the watched keys are modified.

```sh
WATCH mykey
val = GET mykey
val = val + 1
MULTI
SET mykey $val
EXEC
```

* In the above code, if another client modifies the result of val in between WATCH and EXEC, transaction will fail.

## UNWATCH
* `UNWATCH` flushes all the previously watched keys in transaction.