# Pandas

## Overview

[Pandas](https://pandas.pydata.org/) is an open source data analysis and manipulation tool that is ubiquitous across data science and machine learning.

We can install `pandas` using `pip` and then import

```python
import pandas as pd
```

## Data Types

Pandas has two incredibly important data types - the **DataFrame** and the **Series**.

### DataFrames

A DataFrame is a table, with each entry corresponding to a row and column. If we wanted to create a table of individuals and their respective ages and heights:

```python
df = pd.DataFrame({'Age': [22, 27], 'Height': [181, 173]})
```

When printed, it is displayed like this:

```
   Age  Height
0   22     181
1   27     173
```

We can give the rows labels by setting the `index` argument in the constructor

```
df = pd.DataFrame({'Age': [22, 27], 'Height': [181, 173]}, index=['Alice', 'Bob'])
```

```
       Age  Height
Alice   22     181
Bob     27     173
```

### Series

A Series is a sequence of data values - effectively a list. We can create one from a list:

```python
pd.Series([1, 2, 3, 4, 5])
```

```
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

We can give each entry a label and also give the overall Series a name

```
pd.Series([130, 10, 30], index=['Stock ID', 'Quantity', 'Quantity To Buy'], name='Stock Info')
```

We can think of a DataFrame as a load of Series "glued" together, and this will help us when it comes to manipulating data later!

### Using Files

We can read data straight from files using functions such as [`read_csv`](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html), which is very easy to understand. The DataFrame method [`to_csv`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html) allows you to save it afterwards as well. Equivalents exist for other files.
