As the name suggests, data-type specifies the type of the data present in the variable. Variables must be declared with a data-type. 

## 1. Numeric Data types

### Integer Data types
| Data type | Description | Size|Range|
|-----|-----|-----|----|
|uint8|8-bit unsigned integer|1 byte|0 to 255|
|int8|8-bit signed integer|1 byte|-128 to 127|
|int16|16-bit signed integer|2 bytes|-32768 to 32767|
|unit16|16-bit unsigned integer|2 bytes|0 to 65,535|
|int32|32-bit signed integer|4 bytes|-2,147,483,648 to 2,147,483,647|
|uint32|32-bit unsigned integer|4 bytes|0 to 4,294,967,295|
|int64|64-bit signed integer|8 bytes|	-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807|
|uint64|64-bit unsigned integer|8 bytes|0 to 18,446,744,073,709,551,615|

### Float Data types

| Data type | Description | 
|-----|-----|
|float32|32-bit signed floating point number|
|float|64-bit signed floating point number|
|complex32|Number has float32 real and imaginary parts|
|complex64|Number has float32 real and imaginary parts|


## 2. Boolean Data types

| Data type | Description | Size|Range|
|-----|-----|-----|----|
|bool|Stores either true or false|1 byte|True or false|


## 3. String Data types
| Data type | Description | 
|-----|-----|
|string|sequence of characters|

### Example

```c
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	integer uint32 =  1<<32 - 1
	flt float64 = 3.14
    complexNum complex128 = cmplx.Sqrt(8 - 6i)
    str string = "Hello World"
    boolean bool = true
)

func main() {
	fmt.Printf("Value: %v is of Type: %T\n", integer, integer)
	fmt.Printf("Value: %v is of Type: %T\n", flt, flt)
	fmt.Printf("Value: %v is of Type: %T\n", complexNum, complexNum)
	fmt.Printf("Value: %v is of Type: %T\n", str, str)
	fmt.Printf("Value: %v is of Type: %T\n", boolean, boolean)
}
```

### Check result [here](https://onecompiler.com/go/3vpkuw5dc)
