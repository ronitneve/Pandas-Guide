# Pandas-Guide
A Compact to-the-point Pandas Guide with a few tips and tricks!

Pandas has two important parts or Data Structures called _series_ and _Dataframe_

## DataFrame
Pandas dataframe is a Table of data containing ordered list of columns. It's a two-dimensional data-structure that has labels for rows and columns, like a table in an Excel. Values in these columns can be of different datatypes. Both rows and columns in a DataFrame are indexed.

Creating a DataFrame using python dicts.
```
DF = pd.DataFrame(
{"student": ["A", "B", "C", "D"],
   "school":  ["Z", "Y", "Z", "Y"],
   "english": [70, 100, 80, 55],
   "math":    [80, 60, 20, 90],  
   "science": [30, 80, 50, 50]   
  }
)

DF.head()
>
   student school  english  math  science
0       A      Z       70    80       30
1       B      Y      100    60       80
2       C      Z       80    20       50
3       D      Y       55    90       50
```
Pandas dataframe automatically assigns an index value. we can examine the dataframe using ```head()``` method, this method returns first five rows of the df.

Retrieving a column from dataframe
```
DF["math"]
>
0    80
1    60
2    20
3    90
```

Creating a new column.
```
>>> DF['Total'] = DF["english"] + DF['math'] + DF["science"]
>>> DF.head()
   student school  english  math  science  Total
0       A      Z       70    80       30    180
1       B      Y      100    60       80    240
2       C      Z       80    20       50    150
3       D      Y       55    90       50    195
```
Removing columns.
```
>>> DF.drop('Total', axis = 1)
  student school  english  math  science
0       A      Z       70    80       30
1       B      Y      100    60       80
2       C      Z       80    20       50
3       D      Y       55    90       50
```
```drop()``` method has a important parameter 'axis'. By default 'axis' is set to 0 which points to rows, 'axis=1' points to columns. ```drop()``` method does not modify the orignal dataframe it only returns a modified copy, to make the changes to orignal dataframe a parameter 'inplace=True' is used.

Retrieving a row from dataframe
```
>>> DF.loc[2]
student      C
school       Z
english     80
math        20
science     50
Total      150
Name: 2, dtype: object
```
Removing rows using DF.drop()
```
>>> DF.drop(2)
   student school  english  math  science  Total
0       A      Z       70    80       30    180
1       B      Y      100    60       80    240
3       D      Y       55    90       50    195
```
Slicing DataFrames
```
Selecting Columns
>>> DF[['student', 'math']]
  student  math
0       A    80
1       B    60
2       C    20
3       D    90

Selecting Rows form a selection of columns
>>> DF[['student', 'math']].loc[2]
student     C
math       20
Name: 2, dtype: object
```

Slicing using iloc()
"iloc" method uses numerical index to slice rows or columns. Arguments in [rows, columns] format.
```
#Slicing Columns
>>> DF.iloc[:,2:5].head() 
   english  math  science
0       70  80.0     30.0
1      100  60.0     80.0
2       80   NaN     50.0
3       55  90.0      NaN

#Slicing Rows and Columns
>>> DF.iloc[2:,2:5].head()
   english  math  science
2       80   NaN     50.0
3       55  90.0      NaN
```


## Series
 Series is a one dimensional array like object which represents a single column in memory. Pandas Series is like a column in an excel sheet. It has axis label called index. Series can also have a Named or a Datetime index instead of just a numerical index. A Pandas Series is flexible and can have values of different data-structures in the same series.
 
 A series can be created using any array or python dictionary as data. 
```
data = np.array(['a','b','c','d'])
ser = pd.Series(data)
print(ser)

Output:
0   a
1   b
2   c
3   d
dtype: object
```
Series object assigns numeric indexes by default. Using a python dict as data, the _keys_ are used as an index. 

```
data = {'a' : 0., 'b' : 1., 'c' : 2.}
ser = pd.Series(data)
print(ser)

Output:
a 0.0
b 1.0
c 2.0
dtype: float64
```

Just like pandas DataFrame, Series also has multiple sets of functions for data analysis.
```
#Getting the mean of a Series
series_age.mean()
# Getting the size of the Series
series_age.size
# Getting all unique items in a series
series_designation.unique()
# Getting a python list out of a Series
series_name.tolist()
```
## Handling Missing data 

Pandas as an inbuilt ```dropna()``` method. When applied to a dataframe it drops all rows that contain NaN values. ```dropna()``` method does not modify the orignal dataframe by default, 'inplace=True' argument can used to modify the orignal dataframe. ```dropna()``` method can also be used to drop columns containing NaN values by setting 'axis=1' argument.
```
>>> DF
  student school  english  math  science
0       A      Z       70  80.0     30.0
1       B      Y      100  60.0     80.0
2       C      Z       80   NaN     50.0
3       D      Y       55  90.0      NaN

>>> DF.dropna() #Drop rows
  student school  english  math  science
0       A      Z       70  80.0     30.0
1       B      Y      100  60.0     80.0

>>> DF.dropna(axis=1) #Drop columns 
  student school  english
0       A      Z       70
1       B      Y      100
2       C      Z       80
3       D      Y       55
```

Pandas also has a ```fillna()``` method which can replace NaN values with any values we want.
```
>>> DF.fillna("Not Null")
  student school  english      math   science
0       A      Z       70        80        30
1       B      Y      100        60        80
2       C      Z       80  Not Null        50
3       D      Y       55        90  Not Null
```
Usualy missing values are replaced with either The average value of the entire DataFrame or The average value of that row of the DataFrame.
```
>>> DF["math"].fillna(DF["math"].mean())
0    80.000000
1    60.000000
2    76.666667
3    90.000000
Name: math, dtype: float64
```

## Pandas Groupby
Pandas comes with a built-in groupby feature that allows you to group together rows based off of a column and perform an aggregate function on them. Pandas groupby works somewhat like SQL's groupby methods.

Using ```>>> DF.groupby('school')``` will output ```<pandas.core.groupby.generic.DataFrameGroupBy object at 0x10b2f0040>``` string, which indicates that a _groupby_ object is created. 

Once the groupby object has been created, you can call operations on that object to create a new DataFrame.

| Summary Operations | Aggregate Operations |   
|--------------------|----------------------|
| .mean()            | .count()             |
| .sum()             | .max()               |
| .std()             | .min()               |
|.describe()

```
>>> DF.groupby('school').mean()
        english  math  science
school                        
Y          77.5  75.0     80.0
Z          75.0  80.0     40.0
```

Trick to agg string values from col2 using col1 values as a condition
```
df2 = pd.DataFrame(df.groupby("Col1")['col2'].transform(lambda x: ','.join(x)).drop_duplicates())
```

## Concat and Merge DataFrame

pd.concat()
* join : {‘inner’, ‘outer’}, default ‘outer’. 
* ignore_index : by default concat method preserves the index values on the concatenation axis. Set to True to set new numerical index. 
```
>>> DF1
  student school  english  math  science
0       A      Z       70    80       30
1       B      Y      100    60       80

>>> DF2
  student school  english  math  science
0       C      Z       80    20       50
1       D      Y       55    90       50

>>> pd.concat([DF1,DF2], ignore_index=True)
  student school  english  math  science
0       A      Z       70    80       30
1       B      Y      100    60       80
2       C      Z       80    20       50
3       D      Y       55    90       50
```
Inner join 

## Reshape DataFrame
The ```melt``` method in Pandas is one way to convert a _wide_ dataframe to a _long_ one. Lets use a madeup school grades dataframe as an example.
```
#Wide DataFrame
    Student School  english  math  science
0       A      Z       70    80       30
1       B      Y      100    60       80
2       C      Z       80    20       50
3       D      Y       55    90       50
```
```pd.melt()``` method gives us a few options to reshape a DF in the form of parameteres. look at few important ones:
* ```id_vars``` : its a list of Columns used as identifier variables. new columns will have values associated to these 'identifiers'
* ```var_name``` : This is used to create a new column which will indicate which orignal columns the value comes form.
* ```value_name``` : This is used to create a new column which contains all the values or data points.
* ```value_vars``` : List of all columns that we want to transform or pivot. By default all column which are not ```id_vars``` are used.

```
#Reshaping the Data
wideDf.melt(id_vars=["Student", "School"],
             value_vars = ["english", "math", "science"],
             var_name="Subjects",
             value_name="Grades")

#Reshaped Data
   Student School Subjects  Grades
0        A      Z  english      70
1        B      Y  english     100
2        C      Z  english      80
3        D      Y  english      55
4        A      Z     math      80
5        B      Y     math      60
6        C      Z     math      20
7        D      Y     math      90
8        A      Z  science      30
9        B      Y  science      80
10       C      Z  science      50
11       D      Y  science      50
```
We can exclude unimportant or specific columns by tweaking these parameters.
```
#Reshaping the Data
wideDf.melt(id_vars=["Student"],
             value_vars = ["math", "science"],
             var_name="Subjects",
             value_name="Grades")

#Reshaped Data
  student Subjects  Grades
0       A     math      80
1       B     math      60
2       C     math      20
3       D     math      90
4       A  science      30
5       B  science      80
6       C  science      50
7       D  science      50
```

## Pandas Options

```
import pandas as pd 
pd.options.display.max_columns = 50  # None -> No Restrictions
pd.options.display.max_rows = 200    # None -> Be careful with this 
pd.options.display.max_colwidth = 100
pd.options.display.precision = 3
```


## Column Slicing
```
df.iloc[:,2:5].head()             # select the 2nd to the 4th column
df.loc[:,'column_x':].head()   
# select all columns starting from 'column_x'
```

## CSV Reading Tricks

parameters"
header = row number of header (start counting at 0)
skiprows = list of row numbers to skip
index_col = to set index column from csv

df = pd.read_csv('file.csv', usecols=['A', 'C', 'D'], dtype={'D':'category'})

df = pd.read_csv("file.csv", skiprows = lambda x: x > 0 and np.random.rand()> 0.01)