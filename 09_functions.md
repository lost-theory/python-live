# Functions

Functions are reusable pieces of programs. They allow you to give a name to a block of statements, allowing you to run that block using the specified name anywhere in your program and any number of times. This is known as *calling* the function. We have already used a few built-in functions, such as `range`.

The concept of functions is probably *the* most important building block of any non-trivial software (in any programming language), so we will explore various aspects of functions in this chapter.

Functions are defined using the `def` keyword. After this keyword comes an *identifier* name for the function, followed by a pair of parentheses which may enclose some names of variables, and by the final colon that ends the line. Next follows the indented block of statements that are part of this function. For example:

Example (save as `function1.py`):

```python
def say_hello():
    # indented block belonging to the function
    print('hello world')

say_hello()  # call the function
say_hello()  # call the function again
```
Output:

```
$ python function1.py
hello world
hello world
```

**How It Works**

We define a function called `say_hello` using the syntax as explained above. This function takes no parameters and hence there are no variables declared in the parentheses. Parameters to functions are just input to the function so that we can pass in different values to it and get back corresponding results.

Notice that we can call the same function twice which means we do not have to write the same code again.

## Function Parameters

A function can take parameters, which are values you supply to the function so that the function
can *do* something utilising those values. These parameters are just like variables except that the
values of these variables are defined when we call the function and are already assigned values
when the function runs.

Parameters are specified within the pair of parentheses in the function definition, separated by
commas. When we call the function, we supply the values in the same way.

Example (save as `function_param.py`):

```
def compare(a, b):
    if a > b:
        print(a, 'is greater')
    elif a == b:
        print(a, 'is equal to', b)
    else:
        print(b, 'is greater')

# directly pass literal values
compare(3, 4)

# pass variables as arguments
x = 5
y = 7
compare(x, y)
```

Output:

```
$ python function_param.py
4 is greater
7 is greater
```

**How It Works**

Here, we define a function called `compare` that uses two parameters called `a` and `b`.  We find out the greater number using a simple `if..else` statement and then print the bigger number.

The first time we call the function `compare`, we directly supply the numbers as arguments. In the second case, we call the function with variables as arguments. `compare(x, y)` causes the value of argument `x` to be assigned to parameter `a` and the value of argument `y` to be assigned to parameter `b`. The `compare` function works the same way in both cases.

## Local Variables

When you declare variables inside a function definition, they are not related in any way to other variables with the same names used outside the function - i.e. variable names are *local* to the function. This is called the *scope* of the variable. All variables have the scope of the block they are declared in starting from the point of definition of the name.

Example (save as `function_local.py`):

```python
x = 50

def func(x):
    print('x is', x)
    x = 2
    print('Changed local x to', x)

func(x)
print('x is still', x)
```

Output:

```
$ python function_local.py
x is 50
Changed local x to 2
x is still 50
```

**How It Works**

The first time that we print the *value* of the name *x* with the first line in the function's body, Python uses the value of the parameter declared in the main block, above the function definition.

Next, we assign the value `2` to `x`. The name `x` is local to our function.  So, when we change the value of `x` in the function, the `x` defined in the main block remains unaffected.

With the last `print` statement, we display the value of `x` as defined in the main block, thereby confirming that it is actually unaffected by the local assignment within the previously called function.

## The `return` statement

The `return` statement is used to immediately *return* from a function. It is commonly used with a *return value*, which is then available to the caller.

Example (save as `function_return.py`):

```python
def compare(x, y):
    if x > y:
        return x
    elif x == y:
        return 'The numbers are equal'
    else:
        return y

print(compare(2, 3))
```

Output:

```
$ python function_return.py
3
```

**How It Works**

The `maximum` function returns the maximum of the parameters, in this case the numbers supplied to the function. It uses a simple `if..else` statement to find the greater value and then *returns* that value.

Note that a `return` statement without a value is equivalent to `return None`. `None` is a special type in Python that represents nothingness or the "null" value.

Every function implicitly contains a `return None` statement at the end unless you have written your own `return` statement. You can see this by running `print some_function()` where the function `some_function` does not use the `return` statement such as:

```python
def some_function():
    pass
```

Note that the `pass` statement is used to indicate an empty block of statements.

## Docstrings

Python has a nifty feature called *documentation strings*, usually referred to by its shorter name *docstrings*. Docstrings are an important tool that you should make use of since it helps to document your programs better and makes them easier to understand. Amazingly, we can even get the docstring back from, say a function, when the program is actually running!

Example (save as `function_docstring.py`):

```python
def compare(x, y):
    '''
    Prints the maximum of two numbers.
    The two values must be integers.
    '''
    if x > y:
        return x
    elif x == y:
        return 'The numbers are equal'
    else:
        return y


print(compare(3, 5))
print(compare.__doc__)
```

Output:


```
$ python function_docstring.py
5

    Prints the maximum of two numbers.
    The two values must be integers.
```

**How It Works**

A string on the first logical line of a function is the *docstring* for that function. Note that docstrings also apply to [modules](./10_modules.md#modules) and [classes](./13_oop.md#oop) which we will learn about in later chapters.

We can access the docstring of the function by using the `__doc__` (notice the *double underscores*) attribute of the function. Just remember that Python treats *everything* as an object and this includes functions. We'll learn more about objects in the chapter on [classes](./13_oop.md#oop).

If you have used `help()` in Python, then you have already seen the usage of docstrings! What it does is just fetch the `__doc__` attribute of that function and displays it in a neat manner for you. You can try it out on the function above - just include `help(compare)` in your program. Remember to press the `q` key to exit `help`.

Automated tools can retrieve the documentation from your program in this manner. Therefore, I *strongly recommend* that you use docstrings for any non-trivial function that you write. The `pydoc` command that comes with your Python distribution works similarly to `help()` using docstrings.

## Summary

We have learned many aspects of functions but note that we still haven't covered all aspects of them. However, we have already covered most of what you'll use on an everyday basis.
