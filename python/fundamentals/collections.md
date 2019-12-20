There are four types of collections in Python.
## 1. List
List is a collection which is ordered and can be changed. Lists are specified in square brackets.

### Example
```py
mylist=["iPhone","Pixel","Samsung"]
print(mylist)
```

## 2. Tuple
Tuple is a collection which is ordered and can not be changed. Tuples are specified in round brackets.

### Example
```py
myTuple=("iPhone","Pixel","Samsung")
print(myTuple)
```
Below throws an error if you assign another value to tuple again.
```py
myTuple=("iPhone","Pixel","Samsung")
print(myTuple)
myTuple[1]="onePlus"
print(myTuple)
```

## 3. Set
Set is a collection which is unordered and unindexed. Sets are specified in curly brackets.

### Example
```py
myset{"iPhone","Pixel","Samsung"}
print{myset}
```

## 4. Dictionary

Dictionary is a collection of key value pairs which is unordered, can be changed, and indexed. They are written in curly brackets with key - value pairs. 

### Example
```py
mydict = {
    "brand" :"iPhone",
    "model": "iPhone 11"
}
print(mydict)
```
