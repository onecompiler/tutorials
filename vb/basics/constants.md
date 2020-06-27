
Constants are used to represent fixed values which can't be altered later in the code.

## How to declare a constant?

constants are declared using the `Const` keyword.

```c#
[ < attributeList > ] [ accessModifier ] [ Shadows ] Const constantName [ As datatype ] = value
```

* ***attributeList*** − attributeList is optional where you can specify the list of attributes applied to the constants.

* ***accessModifier*** − accessModifier is optional where you specify the accessibility of the constants like  Public, Protected, Private, Friend or Protected Friend.

* ***Shadows*** − shadows is also optional which makes the constant hide a programming element of identical name. 

* ***constantName*** − constantName specifies the name of the constant

* ***datatype*** − datatype specifies the data type of the constant

* ***value*** − value specifies the value assigned to the constant

## Example

```c#
Const TOTAL As Integer = 100
Public Const NAME As String = "One Compiler"
```

Some of the in-built constants are as follows:

| Constant | Description|
|----|----|
| vbCrLf | Carriage return and linefeed |
| vbCr | Carriage return|
| vbLf | Linefeed |
| vbNewLine | Newline |
| vbNullChar | Null character|
| vbNullString | ("") which is used for calling external procedures|
| vbObjectError | Error number|
| vbTab | Tab |
| vbBack | Backspace|
