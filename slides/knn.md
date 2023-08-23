% K-Nearest Neighbors
% CIS 241, Dr. Ladd

# What is K-Nearest Neighbors?

## KNN is both a *regressor* and a *classifier*.

It can predict either numerical values or categories!

## A *machine learning* method based on distances.

## There are many ways to calculate distance.

![[Euclidean distance](http://programminghistorian.org/en/lessons/common-similarity-measures#what-is-similarity-or-distance) is the default in scikit-learn.](img/euclidean.jpg){width=30%}

$$\sqrt[]{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

## The key to KNN is setting the correct *K*.

- In machine learning, *K* or *k* refers to any integer.
- If K is too low, you may be *overfitting*.
- If K is too high, you may be *oversmoothing*.
- You must decide based on the data!

## You can use one-hot encoding to handle factor variables.

How is this different from reference coding?

## You ***must*** standardize your variables.

Also called "normalization", this keeps variables in the same scale.

Not "how much" but "how different from the average."

## The most common standardization is the z-score.

$$z=\frac{x-\bar{x}}{s}$$

The z-score is the original value minus the mean and divided by the standard deviation.

# KNN in Python

## New scikit-learn classes

```python
# For standardization
from sklearn.preprocessing import StandardScaler
# For KNN
from sklearn.neighbors import KNeighborsRegressor, KNeighborsClassifier
```

You will also need plenty of classes and functions that we've used previously!

## Standardizing with z-scores

```python
# Using the penguins dataset
predictors = ["bill_length_mm", "bill_depth_mm", "flipper_length_mm", "sex"]
target = "body_mass_g" # A numerical target for now

# Remove null values and use one-hot encoding
penguins = penguins.dropna()
X = pd.get_dummies(penguins[predictors])
y = penguins[target]

# Split data BEFORE standardizing
X_train, X_test, y_train, y_test = train_test_split(
    X, 
    y, 
    test_size=0.3, 
    random_state=0)

# Standardizing using the training data
scaler = StandardScaler()
scaler.fit(X_train)
X_train_std = scaler.transform(X_train)
```

## Predict a *value* with KNN

```python
# Fit the KNN Regressor
# Choose a sensible value for K (n_neighbors)
knn = KNeighborsRegressor(n_neighbors=5)
knn.fit(X_train_std, y_train)

# Standardize test data before predicting!
X_test_std = scaler.transform(X_test)
predictions = knn.predict(X_test_std)
```

## Predict a *category* with KNN


```python
# First you must split and standardize the data with a new target.
# Decide on your predictors and targets
predictors = ["bill_length_mm", "bill_depth_mm", "flipper_length_mm", "sex"]
target = "species" # A categorical target now

penguins = penguins.dropna()
X = pd.get_dummies(penguins[predictors])
y = penguins[target]

X_train, X_test, y_train, y_test = train_test_split(
    X, 
    y, 
    test_size=0.3, 
    random_state=0)

# Standardizing using the training data
scaler = StandardScaler()
scaler.fit(X_train)

X_train_std = scaler.transform(X_train)
```

## Predict a *category* with KNN (cont.)

```python
# Fit the classification model
# Decide on a good value for K (n_neighbors)
knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train_std, y_train)

# Standardize test data
X_test_std = scaler.transform(X_test)

# Get both probabilities and predictions!
probabilities = knn.predict_proba(X_test_std)
predictions = knn.predict(X_test_std)
```

# Validating a KNN model

## For a KNN Regressor, use residuals.

This works like it would for a linear regression: you can use RMSE to understand how your model performed.

## For a KNN Classifier, use the confusion matrix.

All the usual measures (accuracy, precision, recall, etc.) are valuable here.
