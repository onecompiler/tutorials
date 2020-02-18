Javascript has many in-built methods. Below are some of the string and number methods.

## String Methods

|Method Name| Usage|description|
|----|----|----|
|search()|let sindex=str.search("sub-string");| returns the position of the substring given in the string|
|slice(start,end)|let sub-str=str.slice(starting position,ending position);| slice extracts substring from the given string whose starting poistion is same as mentioned and ending position is end-1|
|substring(start,end|let sub-str=str.substring(starting position,ending position);|substring is similar to slice where it can't accept negative values.|
|substr(start,length)|let sub-str=str.substr(starting position,length);|substr is similar to slice where the second parameter is length instead of end position|
|replace()|let x=str.replace("string to replace","replacement string")|this method is used to replace a particular text with another given text|
|concat()|let str3=str1.concat(" ",str2);|to join two strings|
|trim()|let str1= str.trim();|to remove extra spaces on both sides of the string str|
|charAt()|let c=str.charAt(position);|returns the character present in the position|
|charCodeAt()|let c=str.charCodAt(position);|returns the unicode of the character present in the position|
|split()|let array=str.split("");|to convert string to an array|
|length|let vln = str.length()| Returns length of the string named str|
|indexOf()|let index=str.indexOf("sub-string");| returns the index of first occurence of the substring given|
|lastIndexOf()|let index=str.lastIndexOf("sub-string");|returns the index of last occurence of the substring given|
|toUpperCase()|let str1=str.toUpperCase();|to convert all the letters present in str to upper case|
|toLowerCase()|let str1=str.toLowerCase();|to convert all the letters present in str to lower case|


## Number related Methods


|Method Name| Usage|description|
|----|----|----|
|Number()|Number(x);|to convert to a number, number value of a string is NaN|
|parseInt()|parseInt(x);|to parse string into a integer value, if spaces or any decimal value like `.` is there then first value will be returned|
|parseFloat()|parseFloat(x);|to parse string into a float value|
|toString()|let x = num.toString();|converts number to a string, here num can be a variable or literal or regular expression|
|toFixed()|let x=num.toFixed(no of decimals)|converts number to string by rounding the values based on the no of decimals specified.|
|toExponential()|let x=num.toExponential();|converts number to a string in exponential notation.|
|toPrecision()|let x=num.toPrecision(length)|converts number to string to a specific length based on the length specified.|
|valueOf()|let x= num.valueOf();|returns value of a number, here num can be a variable or literal or regular expression|
