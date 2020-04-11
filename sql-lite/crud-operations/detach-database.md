`DETACH` query is used to detach alias-based database which was previously attached through ATTACH statement 

```sh
DETACH DATABASE 'Alias'
```

Even if a single database is attached with multiple aliases, DETACH will disconnect only with given name.