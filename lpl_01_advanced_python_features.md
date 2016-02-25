## Advanced Python features

A Byte of Python teaches the basic syntax and features of Python. Here is a brief tour of a few more advanced features of the language.

## Short circuiting and conditional expressions

```python
class Person(object):
    def __init__(self, first_name, last_name, nick_name):
        self.first_name = first_name
        self.last_name = last_name
        self.full_name = "{} {}".format(first_name, last_name) if first_name and last_name else ""
        self.nick_name = nick_name

    def get_name(self):
        return self.full_name or self.first_name or self.nick_name or '???'
```

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

So far, when we have been writing and using strings, or reading and writing to a file, we have used only simple characters a..z, A..Z, 0..9, and a few punctuation marks. These are called the "ASCII" characters.

If you want to be able to read and write non-English languages, we need to use the `unicode` type, which is Python's string datatype for implementing the [Unicode standard](https://en.wikipedia.org/wiki/Unicode), which can represent almost every language on Earth (as well as mathematical symbols and things like [emoji](https://en.wikipedia.org/wiki/Emoji#In_the_Unicode_Standard)). Unicode strings start with the character `u`:

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

When we read or write to a file or when we talk to other computers on the internet, we need to convert our unicode strings into a format that can easily be encoded and decoded. One of my most common encoding formats is called "UTF-8". To convert `unicode` to a UTF-8 bytestring (`str`), you use the `unicode.encode` method:

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
* [Pragmatic Unicode talk by Nat Batchelder](http://nedbatchelder.com/text/unipain.html)

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

* `lambda`
* higher-order functions
* `map`
* `reduce`

## Decorators

* `functools.wraps`

## `python -i` and `code.interact`

```bash
$ python -i foo.py
>>>
```

```python
import code; code.interact(local=locals())
```

## `python -mpdb` and `pdb.set_trace`

```bash
$ python -mpdb foo.py
```

```python
import pdb; pdb.set_trace()
```
