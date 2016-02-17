# Operators and Expressions

Most statements (logical lines) that you write will contain _expressions_. A simple example of an expression is `2 + 3`. An expression can be broken down into operators and operands.

_Operators_ are functionality that do something and can be represented by symbols such as `+` or by special keywords. Operators require some data to operate on and such data is called _operands_. In this case, `2` and `3` are the operands.

## Operators

We will briefly take a look at the operators and their usage.

Note that you can evaluate the expressions given in the examples using the interpreter interactively. For example, to test the expression `2 + 3`, use the interactive Python interpreter prompt:

```python
>>> 2 + 3
5
>>> 3 * 5
15
>>>
```

Here is a quick overview of the available operators. First, the arithmetic operators:

- `+` (plus)
    - Adds two objects
    - `3 + 5` gives `8`. `'a' + 'b'` gives `'ab'`.

- `-` (minus)
    - Gives the subtraction of one number from the other; if the first operand is absent it is assumed to be zero.
    - `-5.2` gives a negative number and `50 - 24` gives `26`.

- `*` (multiply)
    - Gives the multiplication of the two numbers or returns the string repeated that many times.
    - `2 * 3` gives `6`. `'la' * 3` gives `'lalala'`.

- `**` (power)
    - Returns x to the power of y
    - `3 ** 4` gives `81` (i.e. `3 * 3 * 3 * 3`)

- `/` (divide)
    - Divide x by y
    - In Python 2, `/` returns integer division: `13 / 3` gives `4`
    - In Python 3, `/` returns float division: `13 / 3` gives `4.333333333333333`

- `//` (divide and floor, integer division)
    - Divide x by y and round the answer _down_ to the nearest whole number
    - `13 // 3` gives `4`
    - `-13 // 3` gives `-5`

- `%` (modulo)
    - Returns the remainder of the division
    - `13 % 3` gives `1`. `-25.5 % 2.25` gives `1.5`.

Comparison operators:

- `<` (less than)
    - Returns whether x is less than y. All comparison operators return the boolean values `True` or `False`. Note the capitalization of these names.
    - `5 &lt; 3` gives `False` and `3 &lt; 5` gives `True`.
    - Comparisons can be chained arbitrarily: `3 < 5 < 7` gives `True`.

- `>` (greater than)
    - Returns whether x is greater than y
    - `5 > 3` returns `True`.

- `<=` (less than or equal to)
    - Returns whether x is less than or equal to y
    - `x = 3; x <= 6` returns `True`

- `>=` (greater than or equal to)
    - Returns whether x is greater than or equal to y
    - `x = 3; x >= 3` returns `True`

- `==` (equal to)
    - Returns whether the objects are equal
    - `x = 2; y = 2; x == y` returns `True`
    - `x = 'str'; y = 'stR'; x == y` returns `False`
    - `x = 'str'; y = 'str'; x == y` returns `True`

- `!=` (not equal to)
    - Returns whether the objects are not equal
    - `x = 2; y = 3; x != y` returns `True`

Boolean operators:

- `not` (boolean NOT)
    - If x is `True`, it returns `False`. If x is `False`, it returns `True`.
    - `x = True; not x` returns `False`.

- `and` (boolean AND)
    - `x and y` returns `False` if x is `False`, else it returns evaluation of y
    - `x = False; y = True; x and y` returns `False` since x is False. In this case, Python will not evaluate y since it knows that the left hand side of the 'and' expression is `False` which implies that the whole expression will be `False` irrespective of the other values. This is called short-circuit evaluation.

- `or` (boolean OR)
    - If x is `True`, it returns True, else it returns evaluation of y
    - `x = True; y = False; x or y` returns `True`. Short-circuit evaluation applies here as well.

## Shortcut for math operation and assignment

It is common to run a math operation on a variable and then assign the result of the operation back to the variable, hence there is a shortcut for such expressions:

```python
a = 2
a = a * 3
```

can be written as:

```python
a = 2
a *= 3
```

Notice that `var = var operation expression` becomes `var operation= expression`.

## Evaluation Order

If you had an expression such as `2 + 3 * 4`, is the addition done first or the multiplication? Our elementary school math class tells us that the multiplication should be done first. This means that the multiplication operator has higher precedence than the addition operator.

Just like the "order of operations" in math, Python has a similar order of operations from the lowest precedence (least binding) to the highest precedence (most binding). This means that in a given expression, Python will first evaluate the operators and expressions lower in the table before the ones listed higher in the table.

For the complete table of operator precedence, see the [Python reference manual](http://docs.python.org/3/reference/expressions.html#operator-precedence). We can also explicitly change the order of evaluation by grouping expressions together with parentheses.

## Changing the Order of Evaluation

To change the order of evaluation and make expressions more readable we can use parentheses to group operations together. For example, `2 + (3 * 4)` is definitely easier to understand than `2 + 3 * 4`, and requires no knowledge of the operator precedence. As with everything else, the parentheses should be used reasonably (do not overdo it) and should not be redundant, as in `(2 + (3 * 4))`.

There is an additional advantage to using parentheses - it helps us to change the order of evaluation. For example, if you want addition to be evaluated before multiplication in an expression, then you can write something like `(2 + 3) * 4`.

## Associativity

Operators are usually associated from left to right. This means that operators with the same precedence are evaluated in a left to right manner. For example, `2 + 3 + 4` is evaluated as `(2 + 3) + 4`.

## Expressions

Example (save as `expression.py`):

```python
length = 5
width = 2

area = length * width
print('Area is', area)
print('Perimeter is', 2 * (length + width))
```

Output:

```
$ python expression.py
Area is 10
Perimeter is 14
```

**How It Works**

The length and width of the rectangle are stored in variables by the same name. We use these to calculate the area and perimeter of the rectangle with the help of expressions. We store the result of the expression `length * width` in the variable `area` and then print it using the `print` function. In the second case, we directly use the value of the expression `2 * (length + breadth)`
in the print function.

## Summary

We have seen how to use operators, operands, expressions, and parentheses for grouping. These are the basic building blocks of any program. Next, we will see how to make greater use of these in our programs using control flow statements.
