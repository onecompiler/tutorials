
IBM transformation extender supports a number of in-built functions. Few of the most used functions are as follows:

## 1. IF

IF evaluates a conditional expression, returning one value if true, another if false.

### Syntax

```
IF (single-condition-expression , result_if_true [ , result_if_false ]) 
```

### Note 
* IF can be used as a best function for returning value only when the field used for single-condition-expression occurrence is only once.
* When the field occurrence is multiple in the input side then LOOKUP or EXTRACT is preferable to do the same functionality.

## 2. FIND

The FIND function looks for one text string within another text string and returns to its starting position, if found.

FIND returns the starting position of the text item specified by text_to_find within the text item specified by where_to_look. A third argument (position_to_start_the_search) can be used to specify the location in where_to_look for the FIND to begin. Bytes in the text are numbered from left to right, with the leftmost byte being position 1.

 Returns a single integer

### Syntax
```
FIND (text_to_find, where_to_look[ , position_to_start_the_search ] ) 
```


### Note

FIND should be used when the searchup string is known and it takes minimum execution time on larger volumes of data.

 
## 3. LOOKUP

The LOOKUP function sequentially searches a series, returning the first member of the series that meets a specified condition.

### Syntax

``` 
LOOKUP (series_to_search , condition_to_evaluate) 
```

### Note

LOOKUP gives good results when size of the data is small


## 4. SEARCHUP

The SEARCHUP function performs a binary search on a series sorted in ASCII ascending order, returning a related object that corresponds to the item found.

The ascending_items_to_search must be sorted in ASCII ascending order. The value to search for is specified as the item_to_match and must be of the same type as the ascending_item_to_search. The object returned (corresponding_object_to_return) must be related to ascending_items_to_search by a common object name.

### Syntax

```
SEARCHUP (corresponding_object_to_return, ascending_items_to_search, item_to_match) 
```
### Note

SEARCHUP is preferable when the data is huge, but the data should be sorted before using SEARCHUP


## 5. SEARCHDOWN

SEARCHDOWN performs a binary search on a series sorted in ASCII descending order, returning a related object that corresponds to the item found.

### Syntax 

```
SEARCHDOWN (corresponding_object_to_return , descending_items_to_search , item_to_match) 
```

## 6. EXTRACT

The EXTRACT function returns all members of a series for which a specified condition is true.

### Syntax

``` 
EXTRACT (objects_to_extract, condition_to_evaluate) 
```
## 7. COUNT
COUNT function is used to return an integer representing the number of valid input or output objects in a series.

### Syntax 

```
COUNT (valid_objects_to_count) 
```

### Note

COUNT function cannot be used twice in a single rule.

## 8. RUN

The RUN function allows you to execute another compiled map from a component or map rule.

### Syntax 

```
RUN (single-text-expression [ , single-text-expression ]) 
```

### Example

```
=VALID (RUN("mapname.mmc", 
-A -GR HANDLEIN(1,PACKAGE(OutputColumData) 
ECHOIN(2, "48") + 
"-IF3X '"+FullFilePath+OutputFileName+"."+Count+"_00n'”+ 
“-IF4R4:5 "
” -OF1+ "+FullFilePath+OutputFileName)+	
"-OE2X –OE3 -WM"),

FAIL(“Mailed failed due to :+ TEXT (LASTERRORCODE ( ) ) + "):" + LASTERRORMSG ( ))
)
```

*	"-A -GR" 
Map will produce no Audit Log and restrictions will be ignored

*	-IF3,-IF3X
 Input Card:3 Source File Override.
*	-IF4R4:5 
IF4 - Input Card:4 Source File Override.
R4:5 - Retry settings. Attempts to access the source 4 times with 5 second intervals

*	-OF1
Output of card1 will get written to a file specified.

*	-OE3, -OE2X
OE3: Echo back output card: 3 output to the calling map
OE2X: with X option, the output of card: 2 is stored in memory and is not echoed back to the calling map 

*	HANDLEIN(1,PACKAGE(OutputColumData)
HANDELIN() is similar to ECHOIN() function, but passes pointers to data instead of echoing data, which reduces validation time.

*	ECHOIN(2, "48") 
The ECHOIN function creates the -IE command option for echoing the data (48) to input 2 of the RUN map.

*	-WM
Sets the WorkSpace work files to reside in memory


## 9. VALID

VALID returns the result of the first argument if it is valid; otherwise, returns the second argument.

### Syntax

```
VALID ( function_that_can_fail , return_value_if_function_fails ) 
```


## 10. FAIL

FAIL function is used  to abort a map based on map or application specific logic. The FAIL function returns "none" to the output to which the function is assigned, aborts the map, and returns message_to_return as the map completion error message included in the execution audit. The map return code will be "30", indicating that the map failed through the FAIL function.


### Syntax
```
FAIL (message_to_return) 
```

## 11. LASTERRORCODE

The LASTERRORCODE function returns a text item whose value is the last error code returned by one of a specified set of functions during map execution.

### Syntax

```
LASTERRORCODE ( ) 
```

## 12.	LASTERRORMSG

The LASTERRORMSG function returns a text item whose value is the message corresponding to the last error code returned by one of a specified set of functions during map execution.

### Syntax
```
LASTERRORMSG ( ) 
```

## 13. HEXTEXTTOSTREAM

This function returns a binary item whose value is the evaluation of the input hex pairs. 

### Syntax

```
HEXTEXTTOSTREAM ( series_of_hex_pairs )
```

## 14. ADDDAYS

This function is to add days to a given date.

### Syntax

```
 ADDDAYS ( any_date , number_of_days_to_add )
```

## 15. ADDMINUTES 

This function is to add minutes to a given date-time, similarly you have ADDHOURS function to add number of hours to a given date-time.

### Syntax

```
 ADDMINUTES ( any_datetime , number_of_minutes_to_add )
 ADDHOURS ( any_datetime , number_of_hours_to_add )
```

## 16. CURRENTDATE

This function is used to return the current date  

### Syntax

```
 CURRENTDATE ( )
```

## 17. CURRENTTIME

This function is used to return the current system time 

### Syntax

```
 CURRENTTIME ( )
```

## 18. CURRENTDATETIME

This function is used to return the current date time.

### Syntax

```
 CURRENTDATETIME ( [ date_time_format_string ] )
```

## 19. DATETONUMBER

This function is used to convert given date into a number.

### Syntax

```
 DATETONUMBER ( date_to_convert )
```

## 20. DATETOTEXT

This function is used to convert given date into text.

### Syntax

```
 DATETOTEXT ( date_to_convert )
```

## 21. TEXTTODATE

This function is used to convert  text which is in CCYYMMDD or YYMMDD format to a date. 

### Syntax

```
 TEXTTODATE ( text_to_convert_to_date )
```

## 22. MAX

This function returns the max number of the specified series.

### Syntax

```
 MAX ( series_of_which_to_find_max )
```

## 23. MIN

This function returns the min number of the specified series.

### Syntax

```
 MIN ( series_of_which_to_find_min )

```

## 24. FROMDATETIME

This function is used to convert given date-time into text.

### Syntax

```
 FROMDATETIME ( date_time [ , date_time_format_string ] )

```

## 25. TODATETIME

This function is used to convert given text into date-time.

### Syntax

```
TODATETIME ( text_to_convert [ , date_time_format_string ] )
```

## 26. PUT

This function is used to send the data to a particular adapater. 

### Syntax

```
 PUT ( adapter_alias , adapter_command , data_to_send_to_adapter )
```

### Examples

### To put data to MQ

```
PUT( "MQS", " -qmn %MQ_MGR%"+" -qn QUEUE.NAME",PACKAGE(data to be sent) )
```

