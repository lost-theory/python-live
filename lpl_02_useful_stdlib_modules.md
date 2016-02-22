## Useful modules in the stdlib

So far we've only covered one module in the standard library: `sys`. There are over 300 modules in Python's standard library. I'm going to cover a few of the ones. But first, a little story.

## A quick example of the stdlib's usefulness

I use Gmail, and one day I wanted to export my entire Gmail history to keep a backup and do some analysis on it (how many emails have I sent? who do I send email to the most? how many emails do I get in a month?).

Gmail has an option to [export your entire mailbox](https://support.google.com/accounts/answer/3024190?source=gsearch&hl=en) as a huge zip file which when extracted becomes an even larger "mbox" file with every message you've ever sent or received. [Mbox](https://en.wikipedia.org/wiki/Mbox) is an old text-based format and it's pretty simple to parse, so I was starting to think about writing my own code to parse it, or to look for an open source tool to do it, but then I thought to myself, maybe Python has a module in the stdlib for this? And it did! In about five minutes I was able to use the built-in [mailbox](https://docs.python.org/2/library/mailbox.html) module to write this:

```python
>>> import mailbox
>>> m = mailbox.mbox("./AllMailIncludingSpamandTrash.mbox", create=False)
>>> my_msgs = [msg for msg in m if 'skryskalla@gmail.com' in msg.get('From', '') and 'skryskalla@gmail.com' in msg.get('To', '')]
>>> len(contents)
2202
```

Which let me answer one of the questions I had, how many emails have I sent to myself? 2202 emails! (I send notes and reminders to my own email address almost every day, there's probably a better way to do this, but it works for me.)

Over the years I've had a number of experiences like that with Python's standard library. It often has just the right mix of tools to let you quickly explore a new problem. Now let's look at a few of those tools.

## `time`

Times and dates are used everywhere in development, and Python has two modules for dealing with them `time` and `datetime`. First up, the [`time`](https://docs.python.org/2/library/time.html) module provides functions for interacting with ["epoch" or "unix" time](https://en.wikipedia.org/wiki/Unix_time), which is the **number of seconds** since January 1st 1970 00:00 UTC. It is a standard way for representing times due to its simplicity: you just store a single number containing the number of seconds. Let's look at a few of the things it can do:

```python
>>> import time
>>> #what is the current unix time (for this machine's current timezone)?
>>> time.time()
1456102199.961139
>>> #what does that look like as a string?
>>> time.ctime()
'Sun Feb 21 16:50:35 2016'
>>> #ctime also accepts a number, let's look at yesterday's date as a string
>>> time.ctime(time.time() - 24*60*60)
'Sat Feb 20 16:51:15 2016'
>>> #what is this machine's timezone offset?
>>> time.timezone #in seconds
28800
>>> time.timezone / (60*60.) #in hours
8.0
```

`time` can also do operations on UTC (also called "GMT") time. It is highly recommended that you use UTC time for your programs, and only convert to/from the user's local timezone when displaying times or receiving times as input. By doing this you can avoid daylight savings issues, or issues with two machines in different timezones producing different dates.

```python
>>> time.gmtime()
time.struct_time(tm_year=2016, tm_mon=2, tm_mday=22, tm_hour=0, tm_min=58, tm_sec=47, tm_wday=0, tm_yday=53, tm_isdst=0)
>>> time.mktime(time.gmtime())
1456131533.0
>>> time.ctime(time.mktime(time.gmtime())) #current UTC time as a string
'Mon Feb 22 00:59:02 2016'
```

`time` also allows you to format and parse times:

```python
>>> #formatting:
>>> time.strftime("%Y", time.localtime())
'2016'
>>> time.strftime("%Y-%m-%d", time.localtime())
'2016-02-21'

>>> #parsing:
>>> time.strptime("2016", "%Y")
time.struct_time(tm_year=2016, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=4, tm_yday=1, tm_isdst=-1)
>>> time.strptime("2016-02-21", "%Y-%m-%d")
time.struct_time(tm_year=2016, tm_mon=2, tm_mday=21, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=6, tm_yday=52, tm_isdst=-1)
```

Finally, one of the most useful functions in `time` is the `sleep` function, which pauses the program for a given number of seconds. It also works with floating point numbers so you can sleep less than a second (e.g. `0.1` = 100 milliseconds).

```python
>>> #wait 5 seconds
>>> time.sleep(5)
>>> #let's prove that it waited for 5 seconds
>>> def f():
...     start = time.time()
...     time.sleep(5)
...     end = time.time()
...     print "I slept for {} seconds".format(end-start)
...
>>> f()
I slept for 5.00017595291 seconds
```

Those are the highlights for the `time` module. Next: `datetime`.

## `datetime`

The `datetime` module gives you a higher level, more object-oriented API for dealing with dates & times instead of dealing with raw # of seconds. There are two main objects within `datetime`: the `datetime` object and the `timedelta` object. Yes, the first object really is named `datetime.datetime`! This is somewhat confusing, so people typically import the two objects this way:

```python
>>> from datetime import datetime, timedelta
```

Now let's use them:

```python
>>> from datetime import datetime, timedelta
>>> #current UTC time vs. local time
>>> datetime.utcnow()
datetime.datetime(2016, 2, 22, 1, 11, 42, 197140)
>>> datetime.now()
datetime.datetime(2016, 2, 21, 17, 11, 47, 529343)
>>> #the `datetime` object has attributes for the year, month, etc...
>>> utcnow = datetime.utcnow()
>>> (utcnow.year, utcnow.month, utcnow.day)
(2016, 2, 22)
>>> #we can also instantiate a datetime object at a moment in time by passing
>>> #year, month, day, hour, min, ... (only year, month, and day are required)
>>> jan_1_2016 = datetime(2016, 1, 1)
>>> jan_1_2016_at_1230_pm = datetime(2016, 1, 1, 12, 30, 0)
```

With the `time` module, when we subtracted two times we were just subtracting two numbers and getting a number of seconds. Let's see what happens when we subtract two `datetime` objects:

```python
>>> jan_1_2016 = datetime(2016, 1, 1)
>>> #how long ago in the past was 2016-01-01?
>>> delta = datetime.now() - jan_1_2016
>>> delta
datetime.timedelta(51, 65634, 957534)
>>> delta.days
51
>>> delta.seconds
65634
>>> delta.microseconds
957534
>>> (delta.days, delta.seconds, delta.microseconds)
(51, 65634, 957534)
>>> str(delta)
'51 days, 18:13:54.957534'
```

It returns a `timedelta` object, which is richer than a pure number of seconds. We can also create our own `timedelta` objects:

```python
>>> #what is a week from today?
>>> datetime.now() + timedelta(days=7)
datetime.datetime(2016, 2, 28, 18, 16, 27, 114208)
>>> #what was the time one week in the past?
>>> datetime.now() - timedelta(days=7)
datetime.datetime(2016, 2, 14, 18, 17, 35, 969333)
```

Finally, `datetime` has the same `strftime` and `strptime` methods as the `time` module, except `strftime` now turns a `datetime` object into a string and `strptime` turns a string into a `datetime`:

```python
>>> #formatting:
>>> datetime.now().strftime("%Y")
'2016'
>>> datetime.now().strftime("%Y-%m-%d")
'2016-02-21'
>>> #there is also one shortcut, isoformat, it returns the datetime in "ISO 8601" format
>>> datetime.now().isoformat()
'2016-02-21T18:30:58.544215'

>>> #parsing:
>>> datetime.strptime("2016-02-21", "%Y-%m-%d")
datetime.datetime(2016, 2, 21, 0, 0)
>>> datetime.strptime("2016", "%Y")
datetime.datetime(2016, 1, 1, 0, 0)
```

As with the `time` module, it is highly recommended to use **UTC** (via `utcnow`) internally within your program and only use local timezones as input/output. One thing the Python standard library lacks is a listing of timezones, so you are required to specify each timezone as a `timedelta` offset. To get a full set of timezones you can install the open source [pytz](https://pypi.python.org/pypi/pytz/) library which lets you to refer to timezones and offsets by name (e.g. `US/Eastern`) and region (e.g. `America/New_York`). The data comes from a standardized set of timezones called the "[tz database](https://en.wikipedia.org/wiki/Tz_database)".

## `random`

Randomness is very useful in programming. It has applications in cryptography, probability, statistics, simulations, games, systems, and [much more](https://en.wikipedia.org/wiki/Applications_of_randomness). Let's walk through the various methods in Python's `random` module:

```python
>>> import random
>>> #`random.random()` will return a floating point number in [0.0..1.0)
>>> #that means starting at 0.0 and up to 1.0, but not including 1.0
>>> random.random()
0.06646262311834161
>>> random.random()
0.6062922783187538
>>> random.random()
0.2782317146377725

>>> #`random.randint(a, b)` will return a random integer in [a..b]
>>> #that includes both `a` and `b`
>>> random.randint(0, 10)
8
>>> random.randint(0, 10)
2
>>> random.randint(0, 10)
7

>>> #`random.choice(seq)` will return a random element from `seq`
>>> random.choice('abc')
'c'
>>> random.choice('abc')
'b'
>>> random.choice('abc')
'c'
>>> random.choice([1,2,3,4,5])
1
>>> random.choice([1,2,3,4,5])
2
>>> random.choice([1,2,3,4,5])
5

>>> #`random.shuffle(seq)` will randomly shuffle all elements in `seq`.
>>> #Think of it as shuffling a deck of cards. It is important to note
>>> #that this happens "in-place", that means it modifies an existing list
>>> #instead of creating a new list.
>>> cards = range(13)
>>> cards
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
>>> random.shuffle(cards)
>>> cards
[4, 6, 9, 10, 11, 1, 8, 12, 2, 0, 3, 7, 5]
>>> random.shuffle(cards)
>>> cards
[6, 7, 4, 1, 12, 9, 2, 8, 0, 3, 11, 5, 10]
>>> #The "in-place" behavior means that you can't shuffle a string.
>>> alphabet = 'abcdefghijklmnopqrstuvwxyz'
>>> random.shuffle(alphabet)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python2.7/random.py", line 291, in shuffle
    x[i], x[j] = x[j], x[i]
TypeError: 'str' object does not support item assignment
>>> #You'd have to convert it to a list first:
>>> alphabet = list('abcdefghijklmnopqrstuvwxyz')
>>> alphabet
['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
>>> random.shuffle(alphabet)
>>> alphabet
['e', 'w', 'p', 'k', 'z', 'g', 'n', 'l', 'i', 'h', 'c', 'b', 't', 'y', 'a', 'o', 'f', 'r', 'v', 'm', 'j', 'u', 's', 'd', 'q', 'x']

>>> #`random.sample(seq, num)` is like shuffle, except it return a random
>>> #`num` elements from the sequence as a new list. The same index will not
>>> #be chosen twice.
>>> random.sample(range(10), 3)
[0, 3, 9]
>>> random.sample(range(10), 3)
[7, 0, 5]
>>> random.sample(range(10), 3)
[2, 6, 9]
>>> random.sample('abcdef', 3)
['f', 'e', 'b']
>>> random.sample('abcdef', 3)
['e', 'b', 'd']
>>> random.sample('abcdef', 3)
['e', 'c', 'd']
```

## `math`

We saw Python's basic mathematical operators such as `+` and `-` and `**` in [Byte of Python Chapter 7: Operators and Expressions](https://github.com/lost-theory/python-live/blob/master/07_op_exp.md). There are more mathematical functions and operations inside the `math` library:

```python
>>> import math
>>> #`ceil(x)` returns the "ceiling" of a float.
>>> #That is the next integer >= x.
>>> math.ceil(1.5)
2.0
>>> math.ceil(-1.5) #remember that -1 is > -1.5
-1.0
>>> math.ceil(1.0)  #1.0 is >= 1.0, so ceil(1.0) is 1.0
1.0
>>> #`floor(x)` returns the "floor" of a float.
>>> #This is the opposite of `ceil`, it is the next integer <= x.
>>> math.floor(1.5)
1.0
>>> math.floor(-1.5) #same behavior as `ceil`, but with <=
-2.0
>>> math.floor(1.0) #same behavior as `ceil`
1.0

>>> #`math.log(x, [base])` will compute the logarithm of `x` using the given `base`.
>>> #The default `base` is `math.e` = 2.71828...
>>> [math.log(10), math.log(100), math.log(1000)]
[2.302585092994046, 4.605170185988092, 6.907755278982137]
>>> #Using base 10 instead:
>>> [math.log(10, 10), math.log(100, 10), math.log(1000, 10)]
[1.0, 2.0, 2.9999999999999996]
>>> #The base 10 logarithm is so common it has its own function:
>>> [math.log10(10), math.log10(100), math.log10(1000)]
[1.0, 2.0, 3.0]

>>> #`math.sqrt(x)` will compute the square root of x.
>>> math.sqrt(64)
8.0
>>> #Note that you can just use x**0.5 for the same result:
>>> 64**0.5
8.0
>>> #This is also how you calculate other roots, for example the cube root:
>>> 64**(1/3.0)
3.9999999999999996

>>> #Trigonometric functions:
>>> #First, `math.pi` is a constant which defines pi, 3.1415926...
>>> math.pi
3.141592653589793
>>> math.pi/2
1.5707963267948966
>>> math.pi*2
6.283185307179586
>>> #We can use it with the trigonometric functions: sin, cos, tan, asin, etc.
>>> [math.sin(math.pi), math.cos(math.pi), math.tan(math.pi)]
[1.2246467991473532e-16, -1.0, -1.2246467991473532e-16]
>>> [math.sin(math.pi/2), math.cos(math.pi/2), math.tan(math.pi/2)]
[1.0, 6.123233995736766e-17, 1.633123935319537e+16]
>>> #Use `degrees(r)` and `radians(d)` to convert between degrees and radians:
>>> math.degrees(math.pi)
180.0
>>> math.radians(180)
3.141592653589793
```

## `string`

Did you know that up until Python 2.0, all the string methods were contained inside the `string` module? For example to write `"foo".replace("o", "a")` you had to write `string.replace("foo", "o", "a")`. That's mostly a historical detail at this point, except that the `string` module still contains some useful constants!

```python
>>> import string
>>> string.lowercase
'abcdefghijklmnopqrstuvwxyz'
>>> string.uppercase
'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
>>> string.letters
'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
>>> string.digits
'0123456789'
>>> string.hexdigits
'0123456789abcdefABCDEF'
>>> string.punctuation
'!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
>>> string.printable
'0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~ \t\n\r\x0b\x0c'
>>> string.whitespace
'\t\n\x0b\x0c\r '
```

## Regex (`re`)

Regular expressions or "regexes" are a powerful method for matching strings against patterns. "Wildcard" patterns such as `*.txt` are a similar idea, except regexes are **much** more powerful. Many languages implement regexes, and Python's regex functionality lives in the `re` module. This document is too small to cover the full regex language, see the [official Python docs on `re`](https://docs.python.org/2/library/re.html#regular-expression-syntax) for details. Instead we will walk through each of the major functions provided by the `re` module.

The first thing to remember about the `re` functions is that the `pattern` is always the **first** argument to the function. **Patterns** are the **most important** thing about regexes, so they always go **first**. Commit it to memory now!

Next, for some of the `search` and `match` examples, we're going to use the following pattern: `"\d{3}-\d{3}-\d{4}"`. This means 3 digits, a "-", 3 digits, another "-", and then 4 digits. This is the pattern for a US phone number, e.g. `800-555-1234`.

```python
>>> #`search` for a pattern, anywhere in a string
>>> #first, a successful search call, this returns a Match object
>>> re.search("\d{3}-\d{3}-\d{4}", "Call now at 1-800-222-4357")
<_sre.SRE_Match object at 0x6ffffbc16b0>
>>> #next, a failed search, this return None
>>> re.search("(\d{3}-\d{3}-\d{4})", "Call now!")
>>>
>>> #`match` a pattern against the *beginning* of a string
>>> #This first `match` fails because even though it contains a phone number,
>>> #it doesn't match at the beginning.
>>> re.match("(\d{3}-\d{3}-\d{4})", "Call now at 1-800-222-4357")
>>> #This will more obviously return None.
>>> re.match("(\d{3}-\d{3}-\d{4})", "Call now!")
>>> #This works, because the entirety of the string matches.
>>> re.match("(\d{3}-\d{3}-\d{4})", "800-222-4357")
<_sre.SRE_Match object at 0x6ffffbe5a08>
>>> #This one will also work, even though there's extra stuff after the match.
>>> re.match("(\d{3}-\d{3}-\d{4})", "800-222-4357 is the phone number to call.")
<_sre.SRE_Match object at 0x6ffffbe5a08>
```

Now, we saw that successful calls returned a "Match" object. What can we do with those?

```python
>>> #the most basic thing you can do is use it in a condition:
>>> if re.search("\d{3}-\d{3}-\d{4}", "Call now at 1-800-222-4357"):
...     print "It has a phone number."
... else:
...     print "No phone number."
...
It has a phone number.
>>>
>>> if re.search("\d{3}-\d{3}-\d{4}", "Call now!"):
...     print "It has a phone number."
... else:
...     print "No phone number."
...
No phone number.

>>> #we can also grab the matched text using the "default" group:
>>> m = re.search("\d{3}-\d{3}-\d{4}", "Call now at 1-800-222-4357")
>>> m.group()
'800-222-4357'

>>> #if we have multiple groups defined by "()":
>>> m = re.search("(\d{3})-(\d{3})-(\d{4})", "Call now at 1-800-222-4357")
>>> #we can also grab the "default" group, the entire matched string:
>>> m.group()
'800-222-4357'
>>> #or we can grab each numbered group:
>>> m.group(1)
'800'
>>> m.group(2)
'222'
>>> m.group(3)
'4357'
>>> #or we can grab all the numbered groups:
>>> m.groups()
('800', '222', '4357')

>>> #lastly, if we used "named" groups defined by "(?P<name>...)"
>>> m = re.search("(?P<begin>\d{3})-(?P<mid>\d{3})-(?P<end>\d{4})", "Call now at 1-800-222-4357")
>>> #we can look up groups by name:
>>> m.group('begin')
'800'
>>> m.group('mid')
'222'
>>> m.group('end')
'4357'
>>> #or get a dictionary of all named groups:
>>> m.groupdict()
{'begin': '800', 'end': '4357', 'mid': '222'}
```

That is about all you need to to use `search` and `match`. Now let's look at the remaining methods: `split`, `findall`, `finditer`, and `sub`:

```python
>>> #First, the `split` method is like `str.split`, except it splits on a pattern.
>>> #Let's say we want to split a string into pieces based on punctuation.
>>> re.split("[.,!()]", "Yeah! Let's do the split. Everyone (that means you).")
['Yeah', " Let's do the split", ' Everyone ', 'that means you', '', '']

>>> #Next, `findall` works like `search`, except instead of returning a single
>>> #match, it will return a list of all the matched strings.
>>> re.findall("\d{3}-\d{3}-\d{4}", "Alice: 1-555-234-1357. Bob: 1-555-641-2468. Pizza place: 1-800-555-9999.")
['555-234-1357', '555-641-2468', '800-555-9999']

>>> #That's handy, but what if we had numbered / named groups? For those we
>>> #want the underlying Match objects. `finditer` will return an iterator
>>> #of the Match objects.
>>> matches = re.finditer("(\d{3})-(\d{3})-(\d{4})", "Alice: 1-555-234-1357. Bob: 1-555-641-2468.")
>>> for m in matches:
...     print m.groups()
...
('555', '234', '1357')
('555', '641', '2468')

>>> #Finally, the `sub` method works a lot like `str.replace`, except it picks
>>> #what to replace based on a pattern.
>>> re.sub("\d{3}-\d{3}-\d{4}", "[REDACTED]", "First I tried 555-234-1357, then 555-641-2468.")
'First I tried [REDACTED], then [REDACTED].'
>>> #There's also a really neat feature where you can pass a function for the `repl`
>>> #argument. This function takes a match object and returns a string. Let's say
>>> #we want to replace each phone number with the corresponding person's name.
>>> re.sub("\d{3}-\d{3}-\d{4}", "[REDACTED]", "First I tried 555-234-1357, then 555-641-2468.")
'First I tried [REDACTED], then [REDACTED].'
>>> def match_to_name(m):
...     number = m.group()
...     if number == '555-234-1357':
...         return 'ALICE'
...     elif number == '555-641-2468':
...         return 'BOB'
...     else:
...         return 'UNKNOWN'
...
>>> re.sub("\d{3}-\d{3}-\d{4}", match_to_name, "First I tried 555-234-1357, then 555-641-2468, then 555-277-9753.")
'First I tried ALICE, then BOB, then UNKNOWN.'
```

## `csv`

CSV (comma separated value) files are common for exchanging tabular data between two systems, especially spreadsheet programs like Excel or Google Sheets, or relational databases. First, let's make some tabular data in Python, an 8x8 multiplication table:

```python
>>> table = []
>>> for i in range(1,9):
...     table.append([])
...     for j in range(1,9):
...         table[-1].append(i*j)
...
>>> table
[[1, 2, 3, 4, 5, 6, 7, 8], [2, 4, 6, 8, 10, 12, 14, 16], [3, 6, 9, 12, 15, 18, 21, 24], [4, 8, 12, 16, 20, 24, 28, 32], [5, 10, 15, 20, 25, 30, 35, 40], [6, 12, 18, 24, 30, 36, 42, 48], [7, 14, 21, 28, 35, 42, 49, 56], [8, 16, 24, 32, 40, 48, 56, 64]]
```

Now let's write it out to a CSV file using `csv.writer`:

```python
>>> import csv
>>> f = open("out.csv", "w")
>>> writer = csv.writer(f)
>>> for row in table:
...     writer.writerow(row)
...
>>> #Note: since we already have all the rows ahead of time, we could have
>>> #just called `writerows` to write everything at once:
>>> #writer.writerows(table)
>>> #`writerow` is better when you're calculating one row at a time.
>>> f.close()
```

Let's see what it looks like:

```python
>>> print open("out.csv").read()
1,2,3,4,5,6,7,8
2,4,6,8,10,12,14,16
3,6,9,12,15,18,21,24
4,8,12,16,20,24,28,32
5,10,15,20,25,30,35,40
6,12,18,24,30,36,42,48
7,14,21,28,35,42,49,56
8,16,24,32,40,48,56,64
```

And let's read it back in using `csv.reader`:

```python
>>> f = open("out.csv")
>>> reader = csv.reader(f)
>>> table2 = list(reader)
>>> table2
[['1', '2', '3', '4', '5', '6', '7', '8'], ['2', '4', '6', '8', '10', '12', '14', '16'], ['3', '6', '9', '12', '15', '18', '21', '24'], ['4', '8', '12', '16', '20', '24', '28', '32'], ['5', '10', '15', '20', '25', '30', '35', '40'], ['6', '12', '18', '24', '30', '36', '42', '48'], ['7', '14', '21', '28', '35', '42', '49', '56'], ['8', '16', '24', '32', '40', '48', '56', '64']]
```

It returned all the numbers as strings! That's a little unfortunate, but it matches up with what the [docs](https://docs.python.org/2/library/csv.html) say: "No automatic data type conversion is performed".  We have to handle our own validation and conversion of the data inside a CSV file.

Now, you may be thinking: this "CSV" stuff looks pretty simple, I can just put a bunch of commas between some strings and get the same result. While that's true for this simple example, CSV has some trickiness when you start including strings with special characters like commas and quotes, or when two different programs have slightly different ways of representing those special characters. The `csv` module handles this trickiness for you. It also has some helpers for reading and writing dictionaries instead of lists (see [`DictReader`](https://docs.python.org/2/library/csv.html#csv.DictReader) and [`DictWriter`](https://docs.python.org/2/library/csv.html#csv.DictWriter)), including automatically picking up on a "header" row.

## Calling other programs (`subprocess`)

Python is very useful as a systems language because it allows you to easily plug different programs together. Some people call it a ["glue" language](https://en.wikipedia.org/wiki/Scripting_language#Glue_languages) for this reason. One common way Python is integrated into other systems is by calling other executables as you would on the command line.

First, a historical note. Over the years, Python has picked up about a dozen different ways to call other programs (`os.system`, `commands.getoutput`, `popen`, `popen2`, `popen3`, `popen4`, ...). You can mostly ignore all these methods and focus on the `subprocess` module, which is the newest method and mostly covers all of the old ways with a more modern interface.

Let's look at some basic usage of `subprocess`. For these examples I will be using two commands: 1. `git` for cloning a git repo, and 2. the unix `ls` command, for printing a list of files. Let's first run them from the command line to start:

```bash
$ git clone https://gist.github.com/0201a10fecff25ae0a12.git ex3
Cloning into 'ex3'...
remote: Counting objects: 15, done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 15 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (15/15), done.
Checking connectivity... done.

$ cd ex3

$ ls
ex3.py  ex3_data.json  ex3_data.py
```

Now let's try running the `git` command in Python using `subprocess`:

```python
>>> result = subprocess.call(["git", "clone", "https://gist.github.com/0201a10fecff25ae0a12.git", "ex3"])
Cloning into 'ex3'...
remote: Counting objects: 15, done.
remote: Compressing objects: 100% (14/14), done.
remote: Total 15 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (15/15), done.
Checking connectivity... done.
>>> result
0
```

There are two things to note. **First**, we had to pass a list of strings to run the command. This is a protection against a vulnerability called "shell injection". By default, the `subprocess` module will only allow a single command to run, and it will try to escape any dangerous input passed into the command. Even in this case where we know the input is safe, we still have to. THere are ways around this, but they should be avoided if possible. **Second**, the other thing to notice is that the `result` returned is `0`. In unix systems, an exit code of `0` means success, while any other number indicates an error (and different numbers represent different kinds of errors). So we know our command was successful in this case.

What happens if we try to run the same command twice? It will throw an error because the directory already exists.

```python
>>> result = subprocess.call(["git", "clone", "https://gist.github.com/0201a10fecff25ae0a12.git", "ex3"])
fatal: destination path 'ex3' already exists and is not an empty directory.
>>> result
128
```

It failed with exit code `128`. So, we can use the exit code to determine success vs. failure. Now let's try the `ls` command:

```python
>>> result = subprocess.call(["ls"], cwd="ex3")
ex3.py  ex3_data.json  ex3_data.py
>>> result
0
```

The thing to note here is that we used a `cwd` parameter to tell it which directory to execute the command in. Normally when you're working on the shell, you `cd` into different directories and run commands on the current directory. The `subprocess` module spawns a brand new shell for every command it runs, so there's no concept of `cd`-ing to a directory between commands and having that state persist between calls.

The exit code is important, but what if we want to grab the output of the command? We can use the `check_output` function instead of `call` to get the output:

```python
>>> out = subprocess.check_output(["ls"], cwd="ex3")
>>> out
'ex3.py\nex3_data.json\nex3_data.py\n'
>>> print out
ex3.py
ex3_data.json
ex3_data.py
```

What do errors look like with `check_output`? Can we get the error output?

```python
>>> out = subprocess.check_output(["git", "clone", "https://gist.github.com/0201a10fecff25ae0a12.git", "ex3"])
fatal: destination path 'ex3' already exists and is not an empty directory.
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python2.7/subprocess.py", line 573, in check_output
    raise CalledProcessError(retcode, cmd, output=output)
subprocess.CalledProcessError: Command '['git', 'clone', 'https://gist.github.com/0201a10fecff25ae0a12.git', 'ex3']' returned non-zero exit status 128
```

Ouch! We got an exception. Let's save the exception object and see what we can do with it:

```python
>>> try:
...     out = subprocess.check_output(["git", "clone", "https://gist.github.com/0201a10fecff25ae0a12.git", "ex3"])
... except subprocess.CalledProcessError, exc:
...     pass
...
fatal: destination path 'ex3' already exists and is not an empty directory.
>>> exc
CalledProcessError()
>>> dir(exc)
[..., 'args', 'cmd', 'message', 'output', 'returncode']
>>> exc.returncode
128
>>> exc.output
''
```

Hmmm... We can see the exit code of `128`, but no error output. This is due to the error message going to `stderr`, while the normal output (which is blank in this case) is going to `stdout`. We can get the error text by **redirecting** `stderr` to `stdout`:

```python
>>> cmd = ["git", "clone", "https://gist.github.com/0201a10fecff25ae0a12.git", "ex3"]
>>> try:
...     out = subprocess.check_output(cmd, stderr=subprocess.STDOUT)
... except subprocess.CalledProcessError, exc:
...     pass
...
>>> exc.output
"fatal: destination path 'ex3' already exists and is not an empty directory.\n"
```

If you want to keep `stdout` and `stderr` separate you need to use the more verbose (and slightly more powerful) [`subprocess.Popen` object](https://docs.python.org/2/library/subprocess.html#subprocess.Popen).

## Working with the filesystem (`os`, `os.path`, `glob`)

In the `subprocess` section above we saw how to run the `ls` command to get a directory listing. We don't need to call out to the shell in order to do this, Python has functionality to read directories and file and much more. Many of these are in the `os` module. First, getting a directory listing:

```python
>>> os.listdir("ex3")
['.git', 'ex3.py', 'ex3_data.json', 'ex3_data.py']
>>> os.listdir("/home/Steven/ex3")
['.git', 'ex3.py', 'ex3_data.json', 'ex3_data.py']
>>> os.listdir("this-doesnt-exist")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
OSError: [Errno 2] No such file or directory: 'this-doesnt-exist'
```

Unlike the `subprocess` module that executed everything in a new shell, the functions in `os` do track your current state, and the `chdir` function is the equivalent to the shell's `cd`:

```python
>>> os.getcwd()
'/home/Steven'
>>> os.chdir("ex3")
>>> os.getcwd()
'/home/Steven/ex3'
>>> os.listdir(".")
['.git', 'ex3.py', 'ex3_data.json', 'ex3_data.py']
```

We can use `os.access` to test the permissions of the given path (`R_OK` = readable, `X_OK` = executable, etc.):

```python
>>> os.access("/usr/bin/python", os.R_OK)
True
>>> os.access("/usr/bin/python", os.X_OK)
True
>>> os.access("/home/Steven/ex3/ex3.py", os.R_OK)
True
>>> os.access("/home/Steven/ex3/ex3.py", os.X_OK)
False
```

We can create a directory with `os.mkdir`:

```python
>>> os.mkdir("foo")
>>> os.listdir(".")
['.git', 'ex3.py', 'ex3_data.json', 'ex3_data.py', 'foo']
```

Rename files and directories using `os.rename`:

```python
>>> os.rename("foo", "foobar")
>>> os.listdir(".")
['.git', 'ex3.py', 'ex3_data.json', 'ex3_data.py', 'foobar']
```

Use `os.remove` to remove a file and `os.rmdir` to remove a directory (note: the directory has to be empty):

```python
>>> os.remove('ex3.py')
>>> os.rmdir('foobar')
>>> os.listdir('.')
['.git', 'ex3_data.json', 'ex3_data.py']
```

Use `os.stat` to look up the "stat" info, e.g. the permissions, owner, creation time, modification time, etc.:

```python
>>> stat = os.stat("ex3_data.py")
>>> stat
posix.stat_result(st_mode=33188, st_ino=7318349394662898, st_dev=4062798683, st_nlink=1, st_uid=197608, st_gid=197121, st_size=417596, st_atime=1456121836, st_mtime=1456121836, st_ctime=1456121836)
>>> oct(stat.st_mode)
'0100644'
>>> stat.st_ctime
1456121836.9084263
>>> time.ctime(stat.st_ctime)
'Sun Feb 21 22:17:16 2016'
```

So far most of these have been pretty low level. The `os` module provides one higher level function called `walk` which can be used to iterate over all the files and directories, recursively, starting in the given directory:

```python
>>> walk.next()
('.', ['.git'], ['ex3_data.json', 'ex3_data.py'])
>>> walk.next()
('./.git', ['hooks', 'info', 'logs', 'objects', 'refs'], ['config', 'description', 'HEAD', 'index', 'packed-refs'])
>>> walk.next()
('./.git/hooks', [], ['applypatch-msg.sample', 'commit-msg.sample', 'post-update.sample', 'pre-applypatch.sample', 'pre-commit.sample', 'pre-push.sample', 'pre-rebase.sample', 'prepare-commit-msg.sample', 'update.sample'])
```

There is also a submodule within `os` called `os.path` which also provides a few higher level functions:

```python
>>> os.path.join("/home/Steven", "ex3")
'/home/Steven/ex3'
>>> os.path.isdir("/home/Steven/ex3")
True
>>> os.path.isfile("/home/Steven/ex3")
False
>>> os.path.isfile("/home/Steven/ex3/ex3_data.py")
True
>>> os.path.exists("/home/Steven/ex3")
True
>>> os.path.exists("/home/Steven/ex3/ex3_data.py")
True
>>> os.path.splitext("ex3_data.py")
('ex3_data', '.py')
>>> os.path.split("/home/Steven/ex3")
('/home/Steven', 'ex3')
```

Lastly, here is one more piece of functionality that commonly used on the command line: wildcard patterns using `glob.glob`. It can be used to match multiple files and directories in the given path:

```python
>>> import glob
>>> glob.glob("*.py")
['ex3.py', 'ex3_data.py']
>>> glob.glob("/home/Steven/ex3/*data*")
['/home/Steven/ex3/ex3_data.json', '/home/Steven/ex3/ex3_data.py']
```

## `SimpleHTTPServer`

The `SimpleHTTPServer` module is used to created a simple web server for the files in your current directory. It's most commonly invoked from the command line with the `python` command's `-m` option, which can be used on some modules to do something useful:

```bash
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
```

Then go to http://127.0.0.1:8000/ in your browser and start clicking around on the files and directories.

## Modules that didn't make the cut

Still hungry for more stdlib modules? Here are a few useful modules that didn't make the cut:

* [itertools](https://docs.python.org/2/library/itertools.html)
* [functools](https://docs.python.org/2/library/functools.html)
* [logging](https://docs.python.org/2/library/logging.html)
* [unittest](https://docs.python.org/2/library/unittest.html)
* [doctest](https://docs.python.org/2/library/doctest.html)
* [collections](https://docs.python.org/2/library/collections.html)
* [decimal](https://docs.python.org/2/library/decimal.html)
* [hashlib](https://docs.python.org/2/library/hashlib.html)
* [tempfile](https://docs.python.org/2/library/tempfile.html)
* [StringIO](https://docs.python.org/2/library/stringio.html)
* [struct](https://docs.python.org/2/library/struct.html)
