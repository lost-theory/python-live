# Operators and Expressions

Most statements (logical lines) that you write will contain _expressions_. A simple example of an expression is `2 + 3`. An expression can be broken down into operators and operands.

_Operators_ are represented by symbols such as `+` or special keywords. Operators require _operands_ to operate on. In the case of `2 + 3`, `2` and `3` are the operands.

## Operators

We will briefly take a look at the operators and their usage.

Note that you can evaluate the expressions given in the examples using the interpreter interactively. For example, to test the expression `2 + 3`, use the interactive Python interpreter prompt:

```python
>>> 2 + 3
5
>>>
```

Here is a quick overview of the available operators. First, the arithmetic operators:

- `+` (plus)
    - Adds two values
    - Numeric addition: `3 + 5` gives `8`, `3 + 5.0` gives `8.0`
    - String addition (concatenation): `'a' + 'b'` gives `'ab'`.
    - You cannot mix strings and integers when using the `+` operator.

- `-` (minus)
    - Subtracts one number from another.
    - `10 - 8` gives `2`.
    - Negative numbers: The `-` sign is used to specify a negative number, e.g. `-5.2`.

- `*` (multiply)
    - Gives the product of two numbers, or returns a string repeated N times.
    - Numeric multiplication: `2 * 3` gives `6`.
    - String repetition: `'la' * 3` gives `'lalala'`.

- `**` (power)
    - Returns x to the power of y (exponentiation)
    - `3 ** 4` gives `81` (i.e. `3 * 3 * 3 * 3`)

- `/` (divide)
    - Divide x by y
    - In Python 2, `/` returns integer division: `13 / 3` gives `4`
    - In Python 3, `/` returns float division: `13 / 3` gives `4.333333333333333`

- `//` (integer division, divide and floor)
    - Divide x by y and round the answer _down_ to the nearest whole number
    - `13 // 3` gives `4`
    - `-13 // 3` gives `-5`

- `%` (modulo)
    - Returns the remainder after division
    - `13 % 3` gives `1`. `-25.5 % 2.25` gives `1.5`.

Comparison operators:

- `<` (less than)
    - Returns whether x is less than y. All comparison operators return the boolean values `True` or `False`.
    - `5 < 3` gives `False` and `3 < 5` gives `True`.
    - Comparisons can be chained together: `3 < 5 < 7` gives `True`.

- `>` (greater than)
    - Returns whether x is greater than y
    - `5 > 3` returns `True`.

- `<=` (less than or equal to)
    - Returns whether x is less than or equal to y
    - if `x = 3`, `x <= 6` will return `True`

- `>=` (greater than or equal to)
    - Returns whether x is greater than or equal to y
    - if `x = 3`, `x >= 3` returns `True`

- `==` (equal to)
    - Returns whether the objects are equal
    - if `x = 2` and `y = 2`, then `x == y` returns `True`
    - if `x = 'foo'` and `y = 'FOO'`, then `x == y` returns `False`
    - if `x = 'foo'` and `y = 'foo'`, then `x == y` returns `True`

- `!=` (not equal to)
    - Returns whether the objects are not equal
    - if `x = 2` and `y = 3`, then `x != y` returns `True`

Boolean operators:

- `not` (boolean NOT)
    - If x is `True`, `not x` returns `False`
    - If x is `False`, `not x` returns `True`.

- `and` (boolean AND)
    - If either `x` or `y` are False, `x and y` returns `False`
    - If `x = False` and `y = True`, `x and y` returns `False` since `x` is `False`. In this case, Python will not evaluate `y` since it knows that the left hand side of the `and` expression is `False`, which makes the whole expression `False` irrespective of the other values. This is called short-circuit evaluation.

- `or` (boolean OR)
    - If either `x` or `y` are True, `x or y` returns `True`
    - If `x = True` and `y = False`, `x or y` returns `True`. Short-circuit evaluation applies here as well.

## Shortcuts for operations and assignment

It is common to run a math operation on a variable and then assign the result of the operation back to the variable. Python's "augmented assignment" operators give you a shortcut for such expressions. Instead of writing:

```python
a = 2
a = a * 3
```

You can use the augmented assignment operator `*=`:

```python
a = 2
a *= 3
```

Notice that `var = var OP expression` becomes `var OP= expression`.

## Evaluation order

If you had an expression such as `2 + 3 * 4`, is the addition done first or the multiplication? Our elementary school math class tells us that the multiplication should be done first. This means that the multiplication operator has higher precedence than the addition operator.

Just like the "order of operations" in math, Python has a similar order of operations from the lowest precedence (least binding) to the highest precedence (most binding). This means that in a given expression, Python will first evaluate the operators and expressions lower in the table before the ones listed higher in the table.

For the complete table of operator precedence, see the [Python reference manual](http://docs.python.org/3/reference/expressions.html#operator-precedence). We can also explicitly change the order of evaluation by grouping expressions together with parentheses.

## Changing the order of evaluation

To change the order of evaluation and make expressions more readable we can use parentheses to group operations together. For example, `2 + (3 * 4)` is easier to understand than `2 + 3 * 4`, and requires no knowledge of the operator precedence. As with everything else, the parentheses should be used reasonably (don't overdo it) and should not be redundant, as in `(2 + (3 * 4))`.

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

**How it works**

The length and width of the rectangle are stored in variables by the same name. We use these to calculate the area and perimeter of the rectangle with the help of expressions. We store the result of the expression `length * width` in the variable `area` and then print it using the `print` function. In the second case, we directly use the value of the expression `2 * (length + breadth)` in the print function.

## Summary

We have seen how to write expressions using operators, operands, and parentheses for grouping. These are the basic building blocks of any program. Next, we will see how to make greater use of these in our programs using control flow statements.
