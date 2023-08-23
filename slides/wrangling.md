% Data Wrangling with pandas
% CIS 241, Dr. Ladd

# Wrangling Data

## Why not just edit the data in the spreadsheet?

## DataFrames are like spreadsheets for Python.

```python
# These are our standard imports
import pandas as pd
import numpy as np
import seaborn as sns
sns.set_theme()
```

```python
taxis = sns.load_dataset('taxis')
taxis
```

## Normally, get data from CSV files.

```python
mydata = pd.read_csv("name_of_file.csv")
```

For today, we're using the built-in taxis data set.

## Every variable (column) has a data type.

- int: integers
- float: floats, doubles, or real numbers
- str: string, character, or object
- Other data types: boolean, date-times, factors

```python
taxis.info() # Pandas works by adding methods to dataframes
```

# Selecting rows and columns

## Brackets let you get a subset of observations (rows).

```python
cheap_fares = taxis[taxis.total < 10]
cheap_fares
```

Remember to save everything in variables

## Row selection uses standard comparisons.

- `>` greater than
- `>=` greater than or equal to
- `<` less than
- `<=` less than or equal to
- `!=` not equal
- `==` equal (note the double equals sign!)

## You can also use logical operators to combine comparisons.

& "and", | "or", and ! "not"

![](img/transform-logical.png)

## Logical operators can also be combined.

```python
extreme_fares = taxis[(taxis.total < 7) | (taxis.total > 50)]
extreme_fares
```

## You try it!

Get only the rows for the green taxis.

## The `.sort_values()` method lets you sort rows by value.

```python
taxis.sort_values("distance", ascending=False)
```

## You can also select a set of variables (columns).

```python
prices = taxis[['fare','tip','tolls','total']]
prices
```

## `.rename()` lets you rename columns.

```python
taxis = taxis.rename(columns={"fare": "base_fare"})
taxis
```

Notice we kept the same variable name here!

## `.assign()` lets you add new columns based on existing ones.

```python
taxis_new_column = taxis.assign(total_per_person = taxis.total/taxis.passengers)
taxis_new_column
```

# Grouping and Summarizing

## We use `.groupby()` with summary statistics to make *summary tables*.

*Summary tables* are new dataframes that summarize our original data.

This paradigm is known as *split-apply-combine*, and it's key to data analysis.

## You can use summary statistic methods to get values for a whole column.

```python
taxis.tip.mean()
```

Stat functions to use: `mean(), median(), min(), max(), std()`.

## Groupby lets you group data, to get summaries for each group.

```
taxis.groupby(['dropoff_borough'])
```

It doesn't look like anything on its own!

## Now we can put it all together!

```python
# Use multiple methods to "chain" operations
taxis.groupby(["dropoff_borough"]).mean()
```

## [Chapter 5](https://wesmckinney.com/book/pandas-basics.html#pandas_frame) and [Chapter 8](https://wesmckinney.com/book/data-wrangling.html) have lots more guidance, and many examples for you to try!
