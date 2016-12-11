## Advanced Python features

A Byte of Python teaches the basic syntax and features of Python. Here is a brief tour of a few more advanced features of the language or things that aren't covered in the book.

## Truthiness

The "truthiness" of a value or expression is its boolean value, e.g. when used in a condition. This is sometimes called the "truth value" or "truthy" / "falsey" value. For example, an empty collection evaluates to `False`, and a collection which contains at least one element evaluates to `True`. You can use this to quickly test if a collection like a `list` is empty or not:

```python
>>> xs = []
>>> if xs:
...     print "it has some elements"
... else:
...     print "it's empty"
...
it's empty
```

You can use the `bool()` call to figure out the truth value of an expression:

```python
>>> #falsey values
>>> bool([])
False
>>> bool({})
False
>>> bool("")
False
>>> bool(0)
False
>>> bool(None)
False

>>> #truthy values
>>> bool(1), bool(-1), bool(1.0), bool(-1.0)
(True, True, True, True)
>>> bool([1,2,3]), bool({1:2}), bool("123")
(True, True, True)
```

## Short circuiting and conditional expressions

The other place where truth values are useful is in "short circuiting" logic, which allows you to use the `and` and `or` operators to pick between values based on how they evaluate as `bool`s:

```python
>>> #`and` will return the first `False`-y value, or the final `True` value
>>> d0 = None
>>> d1 = {}
>>> d2 = {'shape': 'square'}
>>> d3 = {'shape': 'square', 'color': 'blue'}
>>> d0 and d0.get('color')
>>> d1 and d1.get('color')
{}
>>> d2 and d2.get('color')
>>> d3 and d3.get('color')
'blue'

>>> #`or` will return the first `True` value, or the final `False`-y value
>>> d0 or d1
{}
>>> d1 or d0
>>> d0 or d1 or d2 or d3
{'shape': 'square'}
>>> d3 or d2
{'color': 'blue', 'shape': 'square'}
```

There is also a shorter way to write `if` statements in Python. They can be condensed down to a single expression with the `a if b else c` form:

```python
>>> x = 5
>>> "positive" if x >= 0 else "negative"
'positive'
>>> x = -5
>>> "positive" if x >= 0 else "negative"
'negative'
```

Here's an example showing both:

```python
class Person(object):
    def __init__(self, first_name, last_name, nick_name):
        self.first_name = first_name
        self.last_name = last_name
        #this is a conditional expression:
        self.full_name = "{} {}".format(first_name, last_name) if first_name and last_name else ""
        self.nick_name = nick_name

    def get_name(self):
        #this expression relies on short-circuiting logic:
        return self.full_name or self.first_name or self.nick_name or '???'
```

First we use a conditional expression to build the `full_name` attribute, but only if both `first_name` and `last_name` are available. Then in `get_name`, we use short circuiting to fall back to the first truthy value (defaulting to `'???'`):

```python
>>> Person("Steven", "Kryskalla", "stevek").get_name()
'Steven Kryskalla'
>>> Person("Steven", "", "stevek").get_name()
'Steven'
>>> Person("", "Kryskalla", "stevek").get_name()
'stevek'
>>> Person("", "", "stevek").get_name()
'stevek'
>>> Person("", "", "").get_name()
'???'
```

## Bytes vs. unicode

So far, when we have been writing and using strings, or reading and writing to a file, we have used only simple characters a..z, A..Z, 0..9, a few punctuation marks, and a few control characters like `\n`. These are called the "ASCII" characters.

If you want to read and write non-English languages, you need to use the `unicode` type, which is Python's string datatype for implementing the [Unicode standard](https://en.wikipedia.org/wiki/Unicode). Unicode is a database of thouands of characters which can represent almost every language on Earth (as well as mathematical symbols and things like [emoji](https://en.wikipedia.org/wiki/Emoji#In_the_Unicode_Standard)). Unicode strings start with the character `u` and behave just like the byte strings we have been using:

```python
>>> #byte string:
>>> "hello world"
'hello world'
>>> type("hello world")
<type 'str'>

>>> #unicode string:
>>> u"hello world"
'hello world'
>>> type(u"hello world")
<type 'unicode'>
```

When we read or write to a file or when we talk to other computers on the internet, we need to convert our unicode strings into raw bytes. One of the most common encoding formats is called "UTF-8". To convert `unicode` to a UTF-8 bytestring (`str`), you use the `unicode.encode` method:

```python
>>> u"Greek: κόσμε"
u'Greek: \u03ba\u1f79\u03c3\u03bc\u03b5'
>>> u"Greek: κόσμε".encode('utf8')
'Greek: \xce\xba\xe1\xbd\xb9\xcf\x83\xce\xbc\xce\xb5'
```

To go in the opposite direction, from a UTF-8 bytestring (`str`) to unicode, you use `str.decode`:

```python
>>> 'Greek: \xce\xba\xe1\xbd\xb9\xcf\x83\xce\xbc\xce\xb5'.decode('utf8')
u'Greek: \u03ba\u1f79\u03c3\u03bc\u03b5'
```

For more information on encoding and decoding unicode strings vs. byte strings, see the following resources:

* [Python Unicode Howto](http://docs.python.org/2/howto/unicode.html)
* [Pragmatic Unicode talk by Ned Batchelder](http://nedbatchelder.com/text/unipain.html)

## More container types

* `set`
* `collections.Counter`
* `collections.defaultdict`
* `collections.namedtuple`

## Args, kwargs, default values

```python
def g(*args, **kwargs):
    return [args, kwargs]

def f(x=1):
    return x*2
```

## Generators

```python
def f():
    i = 0
    while True:
        yield i
        i += 1
```

More info:
* http://www.jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/

## List comprehensions (and more)

```python
>>> [x*2 for x in range(10)]
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
>>> [x*2 for x in range(10) if x % 2 == 0]
[0, 4, 8, 12, 16]
```

Generator expressions:

```python
>>> (x*2 for x in range(10) if x % 2 == 0)
<generator object <genexpr> at 0x6ffffe79190>
>>> g = _
>>> g.next()
0
>>> g.next()
4
>>> list(g)
[8, 12, 16]
```

Dict comprehensions:

```python
>>> {i: chr(i) for i in range(65, 65+6)}
{65: 'A', 66: 'B', 67: 'C', 68: 'D', 69: 'E', 70: 'F'}
```

Set comprehensions:

```python
>>> [i % 3 for i in range(10)] #just for comparison
[0, 1, 2, 0, 1, 2, 0, 1, 2, 0]
>>> {i % 3 for i in range(10)}
set([0, 1, 2])
```

## Functional programming

* `lambda` allows you to write an "anonymous" function as part of an expression.
* `map` / `reduce` / `filter` are common constructs in FP.
* Higher-order functions: These are functions that either take a function as an argument or return a function (or both).

## Decorators

Decorators are functions which are applied to other functions to alter their behavior. The benefit of using a decorator is that this alteration happens without changing the source code of the underlying function. You can use decorators to do things such as intercepting the underlying function's arguments or return value.

The first step to understanding decorators is to understand that functions can be nested in Python, i.e. a function can define and return a function:

```python
def adder(x):
    def inner(y):
        return x + y
    return inner

>>> adder(5)
<function inner at 0x10c502578>
>>> adder(5)(1)
6
>>> add5 = adder(5)
>>> add5(1)
6
```

A function can also accept a function as an argument and return a function:

```python
def decorator(func):
    return func

def double(x):
    return x*2
    
>>> double(3)
6
>>> decorator(double)
<function double at 0x10c502c80>
>>> decorator(double)(3)
6
>>> double = decorator(double)
>>> double(3)
6
```

Note above how we did `double = decorator(double)` and the function ran exactly the same as it did before. The decorator syntax is just a shortcut for writing this kind of assignment where you wrap a function with another function:

```python
@decorator
def double(x):
    return x*2

### the above syntax is equivalent to: ###

def double(x):
    return x*2
double = decorator(double)
```

Now, if we nest a function inside our new `decorator` function, as we did with `adder` and `inner`, we start to open the door to some interesting possibilities. First, let's create the equivalent of our current do-nothing decorator using a nested function:

```python
def decorator(func):
    def inner(*args, **kwargs):
        return func(*args, **kwargs)
    return inner
```

Now that we have this `inner` function, we have access to 1. the underlying function's arguments, 2. its return value, and 3. the function object itself, `func`. Let's add a print statement before and after we call `func`, as well as inside the `double` function, just to make it clear when each line is executed:

```python
def decorator(func):
    def inner(*args, **kwargs):
        print "Before:", args, kwargs
        result = func(*args, **kwargs)
        print "After:", result
        return result
    return inner

@decorator
def double(x):
    print "Inside:", x
    return x*2

>>> double(10)
Before: (10,) {}
Inside: 10
After: 20
20
```

From here we can control the execution of the wrapped function: we can inspect and alter its arguments or return value, we can choose whether to call the underlying function or not, we could swap it out for a completely different function, we can raise/handle/wrap exceptions, we can add logging or collect timing information, etc.

One reason for using decorators is that since the decorator is independent of the underlying function, it can be re-used across many different functions. Decorators are independent "concerns" that can apply in many places (logging is a classic example).

Another thing that falls out of decorators being independent of the underlying function is that we can stack multiple decorators on top of a single function, to add multiple bits of new behavior. For example, we could add two decorators to a single function, one for validating its arguments, and another for emitting logging information.

One advanced usage of decorators is allowing the decorator to take in its own arguments. They are implemented similarly to the above doubly nested functions, except in order to take in an argument there must be **three** levels of nesting. That third level of nesting is what allows the outermost function to accept an argument and return a new decorator. The [`route`](http://flask.pocoo.org/docs/0.10/api/#flask.Flask.route) method in Flask is a practical example of a decorator which takes arguments.

One final tip: use [`functools.wraps`](https://docs.python.org/2/library/functools.html#functools.wraps) to preserve the metadata of the decorated function (e.g. its docstring).

## `python -i` and `code.interact`

Python's `-i` flag and the `code.interact` function allow you to easily drop into an interpreter for experimenting.

```bash
$ python -i foo.py
>>>
```

```python
import code; code.interact(local=locals())
```

## `python -mpdb` and `pdb.set_trace`

This could be a whole class by itself! Python's debugger is a very powerful tool for troubleshooting errors and closely tracing the execution of your code.

```bash
$ python -mpdb foo.py
```

```python
import pdb; pdb.set_trace()
```
