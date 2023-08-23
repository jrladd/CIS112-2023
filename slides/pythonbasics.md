% Python Basics
% CIS 241, Dr. Ladd

# [A Comprehensive Guide to Python Basics](https://melaniewalsh.github.io/Intro-Cultural-Analytics/02-Python/00-Python.html)

# What's the Difference Between Python and Jupyter?

## Python is a programming *language*.

It's the code you write.

`somevariable = 5 + 6`

## Jupyter is a program, an "integrated development environment" (IDE).

You can write Python in different places, but in this class we will write and run Python inside Jupyter Lab.

![](img/jupyter_blank.png)

## Jupyter Lab lets you see and access files on your computer.

Files are organized in a *hierarchy* of directories.

![](img/jupyter_blank.png)

# Python has its own *syntax*.

## Variables store information

```python
myvar = 5

myvar #or print(myvar)
```

## You Try It!

Create a variable called "newVar" that is equal to the value of five plus seven. Then display your variable to see what its value is.

## Use descriptive variable names, and avoid spaces.

```
i_use_snake_case
otherPeopleUseCamelCase
some.people.use.periods
And_aFew.People_RENOUNCEconvention
```

## Add frequent comments to explain what your code does.

Comments in Python begin with a `#` symbol.

```python
# This variable contains a continuous value
some_variable = 2.5
```

You should also use comments for citations!

## Variables have types.

- String or Character: a piece of text (ex. `"five"`)
- Integer: a discrete numerical value (ex. `5`)
- Float or Double: a continuous numerical value (ex. `5.0`)

```python
stringvar = "five"

type(stringvar)
```

## You can put reusable code in Jupyter Notebooks.

These are ".ipynb" files, and you can create them by clicking the `+` icon at the top left of the Jupyter Lab window and selecting a Python 3 notebook.

Always comment your code so you can remember things when you come back later!

# Lists and Dictionaries contain information.

## A list is surrounded by brackets and can contain any kind of data.

```python
mylist = [5,6,7]

secondlist = ["cat","dog","fish"]

# Access items in a list
mylist[0]
secondlist[1]
```

## A dictionary is surrounded by braces and contains key/value pairs.

```python
mydictionary = {"pet_name": "Fido", "age": 5, "pet_type": "dog"}

# Access items in a dictionary
mydictionary["pet_name"]
mydictionary["age"]
```

# Functions and Libraries store reusable code.

## A function is a command that runs based on some *input* or *parameter*.

Python has many built-in functions.

```python
# Some functions give a number result
sum([5,6,7])
mylist = [5,6,7]
sum(mylist)
len(mylist)

# But functions can do anything! 
type(mydictionary)
```

Functions can do just about anything: calculate values, create graphs, transform data, etc.

## You can create functions like you create variables.

```
def myfunction(arg1, arg2, ... ):
	statements
	return object
```

A real example:

```python
def get_last_value(some_list):
  return some_list[len(some_list) - 1]

# But you can do this with some_list[-1]
```

# Loops and Conditions let you manipulate data.

## You can use the `for` operator to iterate through a list.

```python
mylist = [5,6,7,8]
for item in mylist:
  addone = item + 1
  print(addone)
```

## You can use `if` and `else` to set conditions.

```python
# Let's use the range() function to make a list
newlist = range(1,11) 
for i in newlist:
  if i-5 == 0:
    print("It's five!")
  elif i-5 == 5:
    print("It's ten!")
  else:
    print("Nope, try again...")
```

## Libraries/Packages contain reusable functions made by someone else.

```python
# Install libraries only once in the Terminal 
pip install packagename

# Import a library every time you run your code
import packagename

# You can also rename packages to make it easier
import packagename as pn
```

## You Try It!

Import the `pandas` package and abbreviate it `pd`. If it works, there will be no output!

## All this and more in [Chapters 2 & 3](https://wesmckinney.com/book/python-builtin.html) of Python for Data Analysis!
