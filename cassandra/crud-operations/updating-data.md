UPDATE command is used to update the data in table. Where clause can used to update rows based on condition.


Syntax
```sh
UPDATE <TABLE>  
SET <COLUMN> = <VALUE>  
<COLUMN> = <value>....  
WHERE <condition>   
```

## Update multiple values
```sh
UPDATE <TABLE>  
SET <COLUMN1> = <VALUE1>,  
SET <COLUMN2> = <VALUE2>  
<COLUMN> = <value>....  
WHERE <condition>   
```


### Example

```sh
UPDATE student SET city='New York',name='Rahul'  
WHERE id=2;   
```