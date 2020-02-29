## Why Python is used for data science?

Python is one of the best languages used by data scientists because of it's robust functionalities and libraries. Below are some of the key features which makes Python as a highly suitable language for scientific and research applications.

* ease to use as it has simpler syntax
* great functionalities to deal with mathematics, statistics and scientific calculations.
* Robust Libraries and great community support.
* open-source
* Highly suitable for quick prototyping.
* cross platform and hence python programs can run any where.

## Why you should use NumPy?

NumPy(Numerical Python) is a most powerful Python library which provides various mathematical functions/methods to handle with large dimentional arrays, metrics and Linear algebra. Numpy is an open-source  python library. It is a wrapper around a library implemented in C

Numpy arrays enhance performance and speeds up the execution time as they provide vectorization of mathematical operations. It also provides very useful features for operations on n-arrays and matrices.

Numpy arrays are great alternative to Python Lists. They are very fast and easy to use and also you can perform calculations on entire arrays.

### How to install NumPy

```sh
pip install numpy
```

## NumPy Arrays

One of the most powerful objects is an n-dimensional array type. Arrays is a collection of objects which are of same data type.

we can perform arithmetic functions on NumPy arrays which we cannot do on a list.


### One-dimensional Array

```py
import numpy as np 
arr = np.array([1,2,3,4,5])
```

### Multi-dimensional Array

#### 2-Dimensional Array

```py
import numpy as np 
arr = np.array([[1,2,3],[10,20,30]]) # 2-Dimensional Array
```
#### 3-Dimensional Array

```py
import numpy as np 
arr = np.random.randint(10, size=(3, 5, 6)) # create 3 arrays with 5 rows and 6 columns each.
```
### Examples

```py
import numpy as np 
arr = np.empty([2,3]) # creates 2D array  with 2 rows, 3 columns
arr0 = np.zeros(2) # creates a 1D array with 2 elements whose value is 0.
arr1 = np.ones(2) # creates a 1D array with 2 elements whose value is 1
arr2 = np.asarray([1,2,3]) # creates a 1D array with 3 elements
arr3 = np.random.rand(2,3) # creates an array with 2 rows and 3 columns by using random module for uniformly distributed numbers
```

## Numpy Array methods

### Adding elements

append(array, values) function is used to add elements at the end.

```py
arr = [1,2]
np.append(arr, [3,4,5]) #adds 3,4,5 at the end and gives result as [1,2,3,4,5]
```

### Removing elements

delete(array, value) function is used to delete a particular value from the array

```py
np.delete([1,2,3,4,5], 4) #removes 4 from the array and gives result as [1,2,3,5]
```
### Sorting elements

sort(array, axis, kind, orderby) function is used to sort the array elements.

```py
arr = np.sort([[5,3,1],[7,4,5]], axis=1, kind = 'quicksort')
# gives results as [[1 3 5]
# [4 5 7]]
```
### Join two arrays

```py
x = [1,2,3]
y = [4,5,6]
z = [x,y]
np.concatenate(z)# gives result as: [1 2 3 4]
```
