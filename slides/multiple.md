% Multiple Regression
% CIS 241, Dr. Ladd

# Multiple or *Multivariate* Regression

## Bivariate regression is great at *explanation* but lousy at *prediction*.

## Multiple Regression lets you add more independent variables.

Bivariate regression:

$Y=b_{0}+b_{1}x$

Multivariate regression:

$Y=b_{0}+b_{1}x_{1}+b_{2}x_{2}+b_{3}x_{3}+...$

## You can add more variables as predictors.

Let's stick with the `mpg` dataset from last time.

How would you adjust our bivariate code to accept *multiple* predictors? How would you display coefficients? Try it now!

# How to Choose Predictor Variables

## There's no magic solution! You can try different options, but use your logic and don't just throw everything in there.

## Occam's Razor says that the simplest model is probably the best one.

![Think carefully about how many variables you add.](img/William_of_Ockham.png)

## As you add variables, $R^{2}$ will increase and `RMSE` will decrease.

But think about *how much* it increases or decreases.

## Avoid Multicollinearity: when two predictor variables correlate.

This will confuse the model and mess up your results! It could even result in *false* predictions.

## How do we find multicollinearity?

You can do a *pairwise* comparison of the variables you're thinking about. 

```python
sns.pairplot(cars[predictors], kind='reg')
```

Compare this to the correlation matrix.

```python
cars[predictors].corr()
```

## You can consider categorical variables as predictors, too.

Reference coding converts categorical variables to a set of binary variables.

```python
X = pd.get_dummies(cars[predictors], drop_first=True)
```

The first category should always be left out as the reference (`drop_first=True`). All the remaining slopes are relative to that reference!

## When interpreting coefficients, watch out for *confounding variables*!

Ask yourself: is there an important variable that the data doesn't account for?

## Challenge: Seattle Housing Data

Try to make an effective multivariate linear model to predict housing prices in Seattle.

Take a look at the dataset and logically choose some predictors. Check for multicollinearity before you run your model! When you're done, try to predict housing price based on some new data points you create.

Download [house_sales.tsv](../data/house_sales.tsv). You'll need to open this with:

```python
housing = pd.read_csv("house_sales.tsv", sep="\t")
```

# Regression Diagnostics

## How does the model do on out-of-sample data?

```python
outofsample_fitted = our_model.predict(X_test)

# RMSE
np.sqrt(mean_squared_error(y_test, outofsample_fitted))

# r_2
r2_score(y_test, outofsample_fitted)
```

##  Are the residuals normally distributed, with a mean near 0?

```python
residuals = y_test - outofsample_fitted

sns.displot(x="mpg",data=residuals).set_axis_labels(x="Residuals")
```

You could also use a Q-Q plot for this!

## Does the model suggest heteroskedasticity?

Is the variance consistent across the range of predicted values?

```python
# Plot the absolute value of residuals against the predicted values
sns.regplot(x=outofsample_fitted, y=np.abs(residuals.mpg), lowess=True)
```

If the line is horizontal, there's no heterskedasticity.

## Other validation methods

- Finding Outliers
- Cook's Distance and Leverage
- Check for independence of errors
- Partial residual plots

## Let's try this again with your Seattle housing models!
