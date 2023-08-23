% Neural Networks
% CIS 241, Dr. Ladd
% 🧠🧠🧠

# What is a Neural Network?

AKA Deep Learning, Neural Nets, Artifical Neurons

## Jay Alammar's Visual Guides

[A Visual and Interactive Guide to the Basics of Neural Networks](https://jalammar.github.io/visual-interactive-guide-basics-neural-networks/)

![](img/neuralnets.png){width=60%}

[A Visual And Interactive Look at Neural Network Math](https://jalammar.github.io/feedforward-neural-networks-visual-interactive/)

## Like in linear or logistic regression, neural nets optimize weights (coefficients) and bias (intercept).

## Neural nets often use *gradient descent*.

<small>(Instead of Ordinary Least Squares or other methods.)</small>  
![](img/gradientdescent.png){height=350px}  
<small><a href="https://programminghistorian.org/en/lessons/interrogating-national-narrative-gpt#gradient-descent-explained">Chantal Brousseau in *Programming Historian*</a></small>

## Neural nets have "hidden" layers.

These activation functions allow you to create nonlinear relationships and get more sophisticated predictions.

## Neural nets are always trying to minimize the "loss function."

I.e. we are trying to eliminate as much loss or error as possible. We can add nodes and layers to our network iteratively to do better at the task.

## Neural nets can be supervised or unsupervised.

## Neural nets are ***not*** brains!

# Neural Networks in Python

## We'll focus on SKLearn for now.

`MLPClassifier`: Multi-Layer Perceptron

Other key libraries: [TensorFlow](https://www.tensorflow.org/) and [Keras](https://keras.io/)

## Using the MLPClassifier will be familiar.

- Same workflow as all other sklearn models.
- You must use one-hot encoding and you *must* scale your variables.

```python
from sklearn.neural_network import MLPClassifier
```

## Which hyperparameters are important?

- `hidden_layer_sizes`: number *and* size of hidden layers
- `activation`: type of activation function to use
- `solver`: solving method. 'sgd' and 'adam' are both stochastic gradient descent and useful for larger datasets. 'lbfgs' is better for small data.
- `alpha`: strength of regularization (helps with outliers)

## Hyperparameters, continued

- `learning_rate` and `learning_rate_init`: for gradient descent only, determines the size of the steps
- `max_iter`: the number of iterations or epochs until the model converges
- `random_state`

## Cross-validation lets you compare multiple runs of the model with different training data.

```python
scores = cross_val_score(neural_clf, X, y, cv=5)
print(f"{scores.mean():.2} accuracy with standard deviation {scores.std():.2}")
```

You will need to standardize X first!

## Try it with the Titanic dataset!

1. Load dataset and choose predictors.
2. Wrangle, split, and standardize data.
3. Choose hyperparameters and train neural net.
4. Validate using usual methods.
5. Cross-validate model to see mean accuracy score.

Can you get a cross-validation accuracy above 80%?
