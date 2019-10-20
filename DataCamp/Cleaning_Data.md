# Cleaning Data in Python

## Diagnose data for cleaning

Common data problems

- Inconsistent column names
- Missing data
- Outliers
- Duplicate rows
- Untidy
- Need to process columns
- Column types can signal unexpected data values

Load your data Visualy inspect

```
In [1]: import pandas as pd
In [2]: df = pd.read_csv('literary_birth_rate.csv')
In [3]: df.head()
In [4]: df.tail()
In [5]: df.columns
In [6]: df.shape
In [7]: df.info()
```

## Exploratory data analysis(EDA)

### Frequency counts
Count the number of unique values in our data
```
In [2]: df.continent.value_counts(dropna=False) 
```

### Summary statistics
- Numeric columns

```
In [7]: df.describe()
Out[7]:
female_literacy population
count 164.000000 1.220000e+02
mean 80.301220 6.345768e+07
std 22.977265 2.605977e+08
min 12.600000 1.035660e+05
25% 66.675000 3.778175e+06
50% 90.200000 9.995450e+06
75% 98.500000 2.642217e+07
max 100.000000 2.313000e+09
```


- Outliers
	- Considerably higher or lower
    - Require further investigation

## Visual Exploratory data analysis(EDA)

Data visualization

- Great way to spot outliers and obvious errors
- More than just looking for patterns
- Plan data cleaning steps

Bar plots and histograms

- Bar plots for discrete data counts
- Histograms for continuous data counts
- Look at frequencies

### Histogram
```
In [2]: df.population.plot('hist')
Out[2]: <matplotlib.axes._subplots.AxesSubplot at 0x7f78e4abafd0>
In [3]: import matplotlib.pyplot as plt
In [4]: plt.show()
```

### Box plots

Visualize basic summary statistics

- Outliers
- Min/max
- 25th, 50th, 75th percentiles

```
In [6]: df.boxplot(column='population', by='continent')
Out[6]: <matplotlib.axes._subplots.AxesSubplot at 0x7ff5581bb630>
In [7]: plt.show
```

### Scatter plots

- Relationship between 2 numeric variables
- Flag potentially bad data
	- Errors not found by looking at 1 variable


## Tidy data[二维表转化一维表]

### Principle of tidy data

- Columns represent separate variables
- Rows represent individual observations
- Observational units form tables

### Melt data

```
In [1]: pd.melt(frame=df, id_vars='name',
 ...: value_vars=['treatment a', 'treatment b'])
Out[1]:
 name variable value
0 Daniel treatment a _
1 John treatment a 12
2 Jane treatment a 24
3 Daniel treatment b 42
4 John treatment b 31
5 Jane treatment b 27
```

```
In [2]: pd.melt(frame=df, id_vars='name',
 ...: value_vars=['treatment a', 'treatment b’],
 ...: var_name='treatment', value_name='result')
Out[2]:
 name treatment result
0 Daniel treatment a _
1 John treatment a 12
2 Jane treatment a 24
3 Daniel treatment b 42
4 John treatment b 31
5 Jane treatment b 27
```

### Pivoting data

Pivot: un-melting data

- Opposite of melting
- In melting, we turned columns into rows
- Pivoting: turn unique values into separate columns
- Analysis friendly shape to reporting friendly shape
- Violates tidy data principle: rows contain observations
- Multiple variables stored in the same column


```
In [1]: weather_tidy = weather.pivot(index='date',
   ...: columns='element',
   ...: values='value')
In [2]: print(weather_tidy)
element tmax tmin
date
2010-01-30 27.8 14.5
2010-02-02 27.3 14.4
```

Pivot table

- Has a parameter that specifies how to deal with duplicate
values
- Example: Can aggregate the duplicate values by taking their
average

```
In [5]: weather2_tidy = weather.pivot_table(values='value',
   ...: index='date',
   ...: columns='element',
   ...: aggfunc=np.mean)
Out[5]:
element tmax tmin
date
2010-01-30 27.8 14.5
2010-02-02 27.3 15.4
```

## Combining data for analysis

### Combining rows of data

pandas concat

```
In [4]: pd.concat([weather_p1, weather_p2], ignore_index=True)
Out[4]:
 date element value
0 2010-01-30 tmax 27.8
1 2010-01-30 tmin 14.5
2 2010-02-02 tmax 27.3
3 2010-02-02 tmin 14.4
```

### Finding and concatenationg data

Concatenating many files

- Leverage Python’s features with data cleaning in
pandas
- In order to concatenate DataFrames:
- They must be in a list
- Can individually load if there are a few datasets
- But what if there are thousands?
- Solution: glob function to find files based on a pattern

#### Globbing

- Pattern matching for file names
- Wildcards: * ?
	- Any csv file: *.csv
	- Any single character: file_?.csv
- Returns a list of file names
- Can use this list to load into separate DataFrames

```
In [1]: import glob
In [2]: csv_files = glob.glob('*.csv')
In [3]: print(csv_files)
['file5.csv', 'file2.csv', 'file3.csv', 'file1.csv', 'file4.csv']
In [4]: list_data = []
In [5]: for filename in csv_files:
   ...: data = pd.read_csv(filename)
   ...: list_data.append(data)
In [6]: pd.concat(list_data)
```

### Merge data

```
In [1]: pd.merge(left=state_populations, right=state_codes,
   ...: on=None, left_on='state', right_on='name')
Out[1]:
 state population_2016 name ANSI
0 California 39250017 California CA
1 Texas 27862596 Texas TX
2 Florida 20612439 Florida FL
3 New York 19745289 New York NY
```

## Cleaning data for analysis

### Data types

Converting data type

```
In [2]: df['treatment b'] = df['treatment b'].astype(str)
In [3]: df['sex'] = df['sex'].astype('category')
In [4]: df.dtypes
Out[4]:
name object
sex category
treatment a object
treatment b object
dtype: object
```

Cleaning bad data

```
In [5]: df['treatment a'] = pd.to_numeric(df['treatment a'],
   ...: errors='coerce')
In [6]: df.dtypes
Out[6]:
name object
sex category
treatment a float64
treatment b object
dtype: object
```

### Using regular expressions

- Compile the pattern
- Use the compiled pa!ern to match values
- This lets us use the pa!ern over and over again
- Useful since we want to match values down a column of values

```
In [1]: import re
In [2]: pattern = re.compile('\$\d*\.\d{2}')
In [3]: result = pattern.match('$17.89')
In [4]: bool(result)
True
```

### Using functions to clean data

```
In [5]: import re
In [6]: from numpy import NaN
In [7]: pattern = re.compile('^\$\d*\.\d{2}$')

def diff_money(row, pattern):
	icost = row['Initial Cost']
	tef = row['Total Est. Fee']

	if bool(pattern.match(icost)) and bool(pattern.match(tef)):
		icost = icost.replace("$","")
		tef = tef.replace("$","")

		icost = float(icost)
		tef = float(tef)

		return icost - tef
	else:
		return(NaN)

In [8]: df_subset['diff'] = df_subset.apply(diff_money,
   ...: axis=1,
   ...: pattern=pattern)
 
```

### Duplicate and missing data

```
In [1]: df = df.drop_duplicates()
```
```
In [4]: tips_dropped = tips_nan.dropna() 

In [6]: tips_nan['sex'] = tips_nan['sex'].fillna('missing')
In [7]: tips_nan[['total_bill', 'size']] = tips_nan[['total_bill',
 ...: 'size']].fillna(0)
```

###Testing with asserts

Assert statements

- Programmatically vs visually checking
- If we drop or fill NaNs, we expect 0 missing values
- We can write an assert statement to verify this
- We can detect early warnings and errors
- This gives us confidence that our code is running correctly

```
In [1]: google_0 = google.fillna(value=0)
In [2]: assert google_0.Close.notnull().all()
In [3]: assert google_0.notnull().all().all() 

# The first .all() method will return a True or False for each column, while the second .all() method will return a single True or False
```

### Put all things together

#### Useful methods

```
In [1]: import pandas as pd
In [2]: df = pd.read_csv('my_data.csv')
In [3]: df.head()
In [4]: df.info()
In [5]: df.columns
In [6]: df.describe()
In [7]: df.column.value_counts()
In [8]: df.column.plot('hist')
```

#### Data quality

```
In [9]: def cleaning_function(row_data):
 ...: # data cleaning steps
 ...: return ...
In [10]: df.apply(cleaning_function, axis=1)
In [11]: assert (df.column_data > 0).all()
```

### Combining data

pd.merge

pd.concat()

### Tidy data

Melting turns columns into rows

Pivot will take unique values from a column
and create new columns

### Checking data types

```
In [1]: df.dtypes
In [2]: df['column'] = df['column'].to_numeric()
In [3]: df['column'] = df['column'].astype(str)
```
