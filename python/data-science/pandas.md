Pandas is a open-source library which is built on Numpy for data manipulation by Wes McKinney. Data Frames are the key data structures in Pandas, which allows you to store and manipulate tabular data in rows of observations and columns of variables.

## Why you should use Pandas?

Pandas is a very powerful library which has many features to help data scientists in data manipulation and analysis. some of the key features are as below:

* Handles missing data and data slicing efficiently 
* Uses Series for 1D data structure and DataFrames for multi-dimensional data structures
* Offers flexibility to merge, concatenate or manipulate the data
* Pandas is one of the best solutions to deal with time series data.

## What is a DataFrame

A DataFrame is a two-dimensional data structure means the data is aligned into rows and columns. DataFrames are the standard way to store the data. They are size-mutable, potentially heterogeneous tabular data.

## How to create DataFrame

There are multiple ways to create DataFrames. 

### Using Dictionaties

```py
import pandas as pd
dict1 = {"country": ["USA", "Mexico", "India", "Australia","China", "Indonesia"],
       "language": ["English", "spanish", "Hindi", "English", "Chinese", "Indonesian"]}

df = pd.DataFrame(dict)
print(df)
```
Gives results as below:

```
     country    language
0        USA     English
1     Mexico     spanish
2      India       Hindi
3  Australia     English
4      China     Chinese
5  Indonesia  Indonesian
```
### Using Lists

```py
import pandas as pd
list1 = [1,2,3,4,5,6,7,8,9,10]
df = pd.DataFrame(list1)
print df
```

### import from csv files

You can also import csv files to create DataFrames. Consider you have example.csv stored and can be imported using Pandas using pd.read_csv().

```py
import pandas as pd

data = pd.read_csv('example.csv') # reads example.csv csv file

print(data)
```


