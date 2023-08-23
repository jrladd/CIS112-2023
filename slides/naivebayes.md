% Naive Bayes Classifier
% CIS 241, Dr. Ladd

# What is the Naive Bayes Classifier?

## Not all classifiers are based on a linear model!

## Naive Bayes predicts a target by finding the probability of *predictors* based on a specific target.

Like in hypothesis testing, this is the opposite of what we'd expect!

## Exact Bayesian Classification

Find all records that are the same as the one you care about. What's the proportion of possible targets? The highest proportion is your prediction!

This is impractical, because very few records are identical.

## Naive Bayes Classification

For each possible target, find the individual conditional probabilities of every predictor. Multiply these probabilities by each other and by the number of records in the possible target class. Divide this by the sum of these values for all the classes. That gives you the probability!

## This is *naive* because it assumes every predictor is independent.

This isn't always true, but naive Bayes classifiers can still be useful.

## This can *only* be used with categorical predictor variables!!

Numerical variables would need to be "binned" into categories first.

# Naive Bayes in Python

## We need the `MultinomialNB` class.

```python
from sklearn.naive_bayes import MultinomialNB
```

## Let's return to the Titanic dataset.

Can we predict who survived based on some *categorical* data?

Load the data and create `predictors` and `target` variables.

## Prepare and split the data.

This time we use *one-hot encoding*. We don't need to `drop_first`.

```python
X = pd.get_dummies(titanic[predictors]) # One-hot encoding
y = titanic[target] # Creating a y to keep our code cleaner

X_train, X_test, y_train, y_test = train_test_split(
    X, 
    y, 
    test_size=0.3, 
    random_state=0)
```

## Create and fit the model

```python
naive_model = MultinomialNB(alpha=0.01, fit_prior=True)
naive_model.fit(X_train,y_train)
```

Alpha sets smoothing to prevent problems with 0s. The default is 1, but better to set it smaller. `fit_prior` makes sure to use prior probabilities (True is the default).

## Get probabilities and predictions as usual.

```python
probabilities = naive_model.predict_proba(X_test)
probabilities = pd.DataFrame(probabilities)

predictions = naive_model.predict(X_test)
```

# Validating a Naive Bayes Model

## We use exactly the same methods as every other classifier!

Since we used the Titanic data, we already have our target variable expressed as 0s and 1s.

Import all the same metric functions as last time.

## Visualize the Confusion Matrix

```python
conf_mat = confusion_matrix(y_test,predictions)
(sns.heatmap(conf_mat, 
            cmap='summer', 
            cbar=False, 
            annot=True)
    .set(xlabel="Predicted Response",ylabel="True Response"))
```

## Get Accuracy, Precision, Recall, and Specificity

```python
print(classification_report(y_test, predictions))
```

## Display the ROC Curve

```python
# Create our ROC Curve plot
# This is simpler than last time because
# the category labels are 0 and 1.
# Check the logistic regression slides for
# the more complex method.
RocCurveDisplay.from_predictions(y_test, probabilities[1])

# Draw a green line for 0
plt.plot([0, 1], [0, 1], color = 'g')
```

Remember to pay attention to the AUC score!

## Thanks, Thomas Bayes!

![](img/Thomas_Bayes.gif)
