% Exploratory Data Analysis
% CIS 241, Dr. Ladd

# *Look* at the Data

## John Tukey (1962)

![John Tukey pioneered Exploratory Data Analysis starting in 1962 and again with a book in 1977.](img/tukey.png)

# Summary Statistics

## Summary Statistics are focused on the **location**, **variability**, and **distribution** of data.

# Location: What is the data's "typical value"?

## Let's imagine a variable showing the heights of different dogs.

![The dog's heights (in mm) are: 600, 470, 170, 430, and 300](img/dogs1.gif)

## Mean is the sum of all values divided by the number of values.

AKA "average"

![](img/dogs2.gif)

$\dfrac{600+470+170+430+300}{5} = 394$

## We can calculate the mean easily in Python.

```python
# Put the dog heights into a dataframe
dogs = pd.DataFrame({"height": [600, 470, 170, 430, 300]})

dogs.height.mean() # Calculate the mean
```

## Median is the value such that half of the data lies above and below.

AKA "50th percentile"

![600, 470, 170, 430, 300](img/dogs1.gif)

```python
dogs.height.median()
```

## Percentile is a value such that *P* percent of the data lies below.

AKA "quantile"

## The 25th Percentile is the 1st Quartile.

![600, 470, 170, 430, 300](img/dogs1.gif)

```python
dogs.describe()
```

## The 75th Percentile is the 3rd Quartile.

![600, 470, 170, 430, 300](img/dogs1.gif)

```python
dogs.describe()
```

## The median is the 2nd Quartile!!

## An **outlier** is a data value that's different from most of the data.

AKA "extreme value"

## A **robust** variable is not sensitive to extreme values.

# Variability: Is the data tightly clustered or spread out?

## The **interquartile range** is the difference between the 1st and 3rd quartiles.

```python
dogs.height.quantile(.75)-dogs.height.quantile(.25)
```

## A **deviation** is the difference between an actual value and an estimate of location (like the mean).

![](img/dogs3.gif)

## The **variance** is the sum of the squared deviations, divided by the number of values.

![](img/dogs3.gif)

$\dfrac{206^2+76^2+(-224)^2+36^2+(-94)^2}{5} = 21,704$

## The **standard deviation** is the square root of the variance.

![It gives us a "standard" way of knowing what is normal, or what is extra large/extra small.](img/dogs4.gif)

Rottweilers *are* tall, and dachsunds *are* short---compared to the standard deviation from the mean.

## Now calculate the variance and standard deviations in Python.

```python
dogs.height.var()

dogs.height.std()
```

Were these the results you expected?

## Population vs. Sample

When you have "N" data values:

- The Entire Population: divide by N when calculating variance (like we did)
- A Sample: divide by N-1 when calculating variance

Sample variance: $\dfrac{108,520}{4}=27,130$  
Sample standard deviation: $\sqrt{27,130}=164$

Think of it as a "correction" when your data is only a sample. Pandas does this by default!

## Neither the mean, variance, nor standard deviation are **robust**. They are all very sensitive to outliers!

# Distributions: How many of each value are there?

## Histograms show distributions based on frequency counts.

![The histogram for miles per gallon highway in the mpg dataset.](img/mpg_hwy_hist.png)

## The normal distribution has most values in the middle.

![In a normal distribution, 95% of the values lie within 2 standard deviations of the mean.](img/normal.png)

Be careful: normal distributions are assumed for many statistical analyses!

## Boxplots show distribution based on the median.

![You can see how the box plot and the histogram are similar but different.](img/eda-boxplot.png)

# Correlation: Are two variables related?

## Location, Variability, and Distribution are for one variable at a time (univariate analysis). Correlation is for *two* variables (bivariate analysis).

## Scatter plots and correlation coefficients are used to determine correlation.

We'll talk more about correlation in a couple weeks!

# Resampling and Confidence Intervals

## Resampling is simply drawing multiple random samples from observed data.

I can grab a sample of 5 observations. Then I can "resample" 5 more. Then 5 more, and so on and so on.

## First proposed in the 1960s, resampling procedures weren't practical until computing took off in the 1980s.

## Resampling is an umbrella term. It can include:

- The Bootstrap, used to assess the reliability of an estimated statistic
- Permutation Tests, used as an alternative to parametric hypothesis tests

## You can resample with or without *replacement*.

Replacement means an item is returned to the sample before the next draw (i.e. you could wind up with the same observation multiple times).

## Using bootstrap sampling, we can estimate a confidence interval for any summary statistic.

- Remember: bootstrap sampling is sampling *with* replacement.
- The 90% confidence interval tells us where 90% of the data is likely to fall, if the data were random.
- Confidence intervals are used to gauge *uncertainty*. 

## Confidence intervals with Pandas

```python
n_rows = dogs.shape[0] # First find the number of rows in the dataframe.

# Then take a sample with replacement from your dataset, and
# Calculate the mean each time. Do this 5000 times.
bootstrap_samples = [dogs.sample(n=n_rows, replace=True).height.mean()
                     for i in range(5000)]
# Put the results into a pandas Series object
bootstrap_samples = pd.Series(bootstrap_samples)

# Calculate the 95th percentile and the 5th percentile.
top_percentile = bootstrap_samples.quantile(.95)
bottom_percentile = bootstrap_samples.quantile(.05)

# Print the results using nice f-strings.
print(f"""The mean dog height in our data is {dogs.height.mean():.3f},
      with a 90% confidence interval
      of {bottom_percentile:.3f} to {top_percentile:.3f}.""")
```

# Visualization

![xkcd.com/688](img/xkcd_self_description.png)

## Some Good Resources on Visualization

- Claus Wilke's [Fundamentals of Data Visualzation](https://clauswilke.com/dataviz/) (The illustrations in this slide show come from here!)
- [The Seaborn tutorials](https://seaborn.pydata.org/tutorial.html)

# Why Do We Visualize Data?

## Visualization can be *exploratory*, *explanatory*, or both!

- Exploratory viz helps *us* (the researchers or analysts) understand the data.
- Explanatory viz helps *others* (the clients or audience) understand our analysis.
- Many visualizations do both of these things at once!

# What Does Visualization Help Us to See?

## Viz Can Help Us See *Amounts*.

![Graph a single value across one set of categories.](img/amounts-1.png)

- Variable types: 1 categorical and 1 numerical
- Common graph types: Bar plot (Don't confuse the dot plot with the scatter plot!)

## Plot *Amounts* With Multiple Categories.

![](img/amounts-2.png)

- Variable types: 2 or more categorical, 1 numerical
- Common graph types: Grouped or stacked bar, heat map

## Viz Can Help Us See *Distributions*.

![Graph a distribution of a single variable.](img/single-distributions.png)

- Variable type: 1 continuous (numerical)
- Common graph types: Histograms, Density plots, Q-Q plots

## *Distributions* with Multiple Categories.

![](img/multiple-distributions.png)

- Variable types: 1 continuous (numerical), 1 categorical
- Common graph types: Box plots, Violin plots

## You Try It!

Look at the `taxis` data set. What visualization type would you use to compare the counts of each destination Borough? Which variables would you use, and what *kind* of variables are they? Jot down your answers.

## Viz Can Help Us See *Proportions*.

![](img/proportions.png)

- Variable types: 1 numerical, 1 categorical
- Common graph types: Pie chart, Bar plot

## Viz Can Help Us See *Relationships*.

![](img/basic-scatter.png)

- Variable types: 2 continuous (numerical), (3 in a bubble chart)
- Common graph types: Scatter plot, Bubble Chart, Hex bins, Density contours

## You Try It!

Look at the `taxis` data again. What visualization type would you use to compare the distribution of tips among different taxi colors? Which variables would you use, and what *kind* of variables are they? Jot down your answers.

## Viz Helps Us See *Time*, *Location*, *Uncertainty*...

![Confidence bands are an example of visualized uncertainty.](img/confidence-bands.png)

More on these viz types in future lessons!

# Make Great Viz with `seaborn`

## Import Seaborn and add default theme

```python
import seaborn as sns
```

```python
sns.set_theme()
# It's a good idea to use this every time
```

## Each Seaborn function looks for a data parameter and variables

```python
# A scatter plot as an example
sns.relplot(x="FirstVariable",y="SecondVariable",data=YourDataFrame)
```

In this example, `FirstVariable` becomes the x-axis and `SecondVariable` becomes the y-axis. You can also add a mapping for `hue` (i.e. color).

## Create different shapes and plot types with different functions.

- `sns.relplot()`: scatter plots and line plots (relationships)
- `sns.displot()`: histograms and frequency polygons (distributions)
- `sns.catplot()`: bar plots and boxplots (categories)
- and more we'll cover later!

Sometimes you'll use the `kind` parameter to set the type of plot!

## You Try It!

Create a plot to compare the distance a taxi traveled to the total fare using the `taxis` DataFrame. Then create the same plot but show color as the type of payment.

## Use the `col` parameter to create side-by-side plots for different categories.

```python
sns.displot(x="NumericalVariable", col="CategoricalVariable", data=YourDataFrame)
```

## Use `.set_axis_labels()` to add your own graph labels.

Never rely on the default column names! You can "chain" this function onto an existing Seaborn plot.

```python
sns.catplot(...).set_axis_labels("A Better Label for X", "A Better Label for Y")
```

## You Try It!

Create a plot showing the distribution of tips in the `taxis` dataset. Then show the distributions according to each pickup borough, in different columns. Finally, change the size of the bins in each distribution to show less detail (wider bars).

# Ugly, Bad, or Wrong 

---

<img src="img/audible_bar.png" style="max-height:80vh" />

---

<img src="img/spreadsheet_viz.jpg" style="max-height:80vh" />

---

<img src="img/german_map.png" style="max-height:80vh" />

---

<img src="img/portuguese_elections.png" style="max-height:80vh" />

---

<img src="img/evaporation_color.jpg" style="max-height:80vh" />

---

<img src="img/violentcrime.jpg" style="max-height:80vh" />

## More Examples of Bad Viz

[viz.wtf](https://viz.wtf/)

[r/dataisugly](https://www.reddit.com/r/dataisugly/)
