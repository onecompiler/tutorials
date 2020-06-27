Date is a built-in javascript object. It stores date and time and allows methods to play around with the date and time format we want.

### Syntax

You can create a new date object using `new Date()`.

```javascript
new Date();
```
### Example
```javascript
let now = new Date();
console.log(now); // prints Date and time in the format 2020-02-21T06:02:08.516Z
```

## To print date in the local format

To  create date in local format where year should be 4 digits and months starts from 0.

### Syntax

```javascript
new Date(year, month, date, hours, minutes, seconds, ms)
```

### Example

```javascript
let now =new Date(2020, 0, 1, 0, 0, 0, 0); 
console.log(now); // prints 2020-01-01T00:00:00.000Z
```

## Date Methods

considering date = new Date() for the below methods.

|Method| 	Description| Usage|
|----|----|----|
|getDay() |	to get the day of the week as a number (0-6)| date.getDay()|
|Date.now()| 	to get the date and time.|let now = Date.now()|
|getFullYear() | to	get the year as a four digit number (yyyy)| date.getFullYear();|
|setFullYear() | to	set the year (optionally you can add month and day)|let year = date.setFullYear(2020);|
|getMonth() | to get the month as a number (0-11)|date.getMonth();|
|setMonth() | to set the month (0-11)| let month = date.setMonth(10);|
|getDate() | to	get the day as a number (1-31)|date.getDate();|
|setDate() | to	set the day as a number (1-31)|let day = date.setDate(20);|
|getHours() | to get the hour (0-23)|date.getHours();|
|setHours() | to set the hour (0-23)|let hrs = date.setHours(20);|
|getMinutes()|  to get the minute (0-59)|date.getMinutes();|
|setMinutes() |	to set the minutes (0-59)| let min = date.setMinutes(40);|
|getSeconds() |	to get the second (0-59)|date.getSeconds(); |
|setSeconds() |	to set the seconds (0-59)| let sec = date.setSeconds(30);|
|getMilliseconds()| to 	get the millisecond (0-999)|date.getMilliseconds();|
|setMilliseconds() | to	set the milliseconds (0-999)|let milli = date.setMilliseconds(500);|
|setTime() | to	set the time (milliseconds since January 1, 1970)|let dateTime = date.setTime(1582268856705);|
|getTime() |  to get the time (milliseconds since January 1, 1970)|date.getTime()|

