# Basics

Just printing `hello world` is not enough, is it? You want to do more than that - you want to take some input, manipulate it and get something out of it. We can achieve this in Python using constants and variables, and we'll learn some other concepts as well in this chapter.

## Comments

_Comments_ are any text to the right of the `#` symbol and is mainly useful as notes for the reader of the program.

For example:

```python
print('hello world') # show a greeting
```

or:

```python
# Show a greeting
print('hello world')
```

Use as many useful comments as you can in your program to:

- explain assumptions
- explain important decisions
- explain important details
- explain problems you're trying to solve
- explain problems you're trying to overcome in your program, etc.

[*Code tells you how, comments should tell you why*](http://www.codinghorror.com/blog/2006/12/code-tells-you-how-comments-tell-you-why.html).

This is useful for readers of your program so that they can easily understand what the program is doing. Remember, that person can be yourself after six months!

## Literal Constants

An example of a literal constant is a number like `5`, `1.23`, or a string like `'This is a string'` or `"It's a string!"`.

It is called a literal because it is _literal_ - you use its value literally. The number `2` always represents itself and nothing else - it is a _constant_ because its value cannot be changed. Hence, all these are referred to as literal constants.

## Numbers

Numbers are mainly of two types - integers and floats.

An examples of an integer is `2` which is just a whole number.

Examples of floating point numbers (or _floats_ for short) are `3.23` and `52.3E-4`. The `E` notation indicates powers of 10. In this case, `52.3E-4` means `52.3 * 10^-4`.

> **Note for Experienced Programmers**
>
> The `int` type can be an integer of any size, you don't need to worry about a separate `long` type.

## Strings

A string is a _sequence_ of _characters_. Strings are basically just a bunch of words.

You will be using strings in almost every Python program that you write, so pay attention to the following part.

### Single Quote

You can specify strings using single quotes such as `'Quote me on this'`.

All white space i.e. spaces and tabs, within the quotes, are preserved as-is.

### Double Quotes

Strings in double quotes work exactly the same way as strings in single quotes. An example is `"What's your name?"`.

### Triple Quotes

You can specify multi-line strings using triple quotes - (`"""` or `'''`). You can use single quotes and double quotes freely within the triple quotes. An example is:

```python
'''This is a multi-line string. This is the first line.
This is the second line.
"What's your name?," I asked.
He said "Bond, James Bond."
'''
```

### Strings Are Immutable

This means that once you have created a string, you cannot change it. Although this might seem like
a bad thing, it really isn't. We will see why this is not a limitation in the various programs that
we see later on.

> **Note for C/C++ Programmers**
> 
> There is no separate `char` data type in Python. There is no real need for it and I am sure you won't miss it.

<!-- -->

> **Note for Perl/PHP/Ruby Programmers**
> 
> Remember that single-quoted strings and double-quoted strings are the same - they do not differ in any way.

### The format method

Sometimes we may want to construct strings from other information. This is where the `format()` method is useful.

Save the following lines as a file `str_format.py`:

```python
age = 20
name = 'Swaroop'

print('{0} was {1} years old when he wrote this book'.format(name, age))
print('Why is {0} playing with that python?'.format(name))
```

Output:

```
$ python str_format.py
Swaroop was 20 years old when he wrote this book
Why is Swaroop playing with that python?
```

**How It Works**

The `{}` patterns within the strings above are used as placeholders for the `format` method to substitute values into the string.

Observe the first usage where we use `{0}`, it corresponds to the variable `name` which is the first argument to the format method. Similarly, the second specification is `{1}` corresponds to `age`, which is the second argument to the format method. Note that Python, like many other programming languages, starts counting from 0 which means that first position is at index 0, second position is at index 1, and so on.

Notice that we could have achieved the same using string concatenation:

```python
name + ' is ' + str(age) + ' years old'
```

But that is much uglier and error-prone: 1. The conversion of the variables into strings is done automatically by the `format` method, so we can avoid the explicit conversion to strings using `str`. 2. The `format` method allows us to cleanly separate out the message as a single string with placeholders, and the values as a list of items, without needing to manually join everything together with `+`.

Also note that the numbers are optional, so you could have also written as:

```python
age = 20
name = 'Swaroop'

print('{} was {} years old when he wrote this book'.format(name, age))
print('Why is {} playing with that python?'.format(name))
```

This will give the same exact output as the previous program.

What Python does in the `format` method is that it substitutes each argument value into the place of the specification. There can be [more detailed specifications](https://docs.python.org/3/library/string.html#formatexamples) such as:

```python
# decimal (.) precision of 3 for float '0.333'
print('{:.3f}'.format(1.0/3))
# fill in 20 characters with underscores (_), then center the text inside
print('{:_^20}'.format('hello'))
# using keywords
print('{name} wrote {book}'.format(name='Swaroop', book='A Byte of Python'))
```

Output:

```
0.333
_______hello________
Swaroop wrote A Byte of Python
```

Since we are discussing formatting, note that `print` always ends with an invisible "new line" character (`\n`) so that repeated calls to `print` will all print on a separate line each. To prevent this newline character from being printed, you can specify that it should `end` with a blank:

```python
#Python 3:
print('a', end='')
print('b', end='')

#Python 2:
#This is not easily do-able with Python 2's "print" statement. See http://stackoverflow.com/a/493399.
```

Output is:

```
ab
```

Or you can `end` with a space:

```python
#Python 3:
print('a', end=' ')
print('b', end=' ')
print('c')

#Python 2:
print 'a',
print 'b',
print 'c'
```

Output is:

```
a b c
```

### Escape Sequences

Suppose, you want to have a string which contains a single quote (`'`), how will you specify this string? For example, the string is `"What's your name?"`. You cannot specify this string as `'What's your name?'` because Python will be confused as to where the string starts and ends. So, you will have to specify that this single quote does not indicate the end of the string. This can be done with the help of what is called _escaping_ or using an _escape sequence_. You specify the single quote with a backslash in front of it as `\'`. Now, you can specify the string as `'What\'s your name?'`.

Another way of specifying this specific string would be `"What's your name?"` i.e. using double quotes. Similarly, you have to use an escape sequence for using a double quote itself in a double quoted string. Also, to use the `\` character itself you must escape the backslash itself with another backslash: `\\`.

What if you wanted to specify a two-line string? One way is to use a triple-quoted string as shown [previously](#triple-quotes) or you can use an escape sequence for the newline character - `\n` to indicate the start of a new line. An example is:

```python
'This is the first line\nThis is the second line'
```

Another useful escape sequence to know is the tab: `\t`. There are many more escape sequences but I have mentioned only the most useful ones here.

One thing to note is that in a string, a single backslash at the end of the line indicates that the string is continued in the next line, but no newline is added. For example:

```python
"This is the first sentence. \
This is the second sentence."
```

is equivalent to

```python
"This is the first sentence. This is the second sentence."
```

### Raw Strings

If you need to specify some strings where no special processing such as escape sequences are handled, then what you need is to specify a _raw_ string by prefixing `r` to the string. For example:

```python
r"Newlines are indicated by \n"
r"c:\Users\Foo"
```

> **Note for Regular Expression Users**
> 
> Raw strings are especially useful when dealing with regular expressions. Otherwise, a lot of escaping may be required. For example, backreferences can be referred to as `'\\1'` or `r'\1'`.

## Variables

Using just literal constants can soon become boring - we need some way of storing any manipulating these values. This is where _variables_ come into the picture. Variables are exactly what the name implies - their value can vary, i.e., you can store anything using a variable. Variables are just parts of your computer's memory where you store some information. Unlike literal constants, you need some method of accessing these variables and hence you give them names.

## Identifier Naming

Variables are examples of identifiers. _Identifiers_ are names given to identify _something_. There are some rules you have to follow for naming identifiers:

- The first character of the identifier must be a letter of the alphabet or an underscore (`_`).
- The rest of the identifier name can consist of letters, underscores (`_`), or digits (0-9).
- Identifier names are case-sensitive. For example, `myname` and `myName` are _not_ the same. Note the lowercase `n` in the former and the uppercase `N` in the latter.
- Examples of _valid_ identifier names are `i`, `name_2_3`. Examples of _invalid_ identifier names are `2things`, `this is spaced out`, `my-name` and `>a1b2_c3`.

## Data Types

Variables can hold values of different types called _data types_. The basic types are numbers and strings, which we have already discussed. In later chapters, we will see how to create our own types using [classes](./13_oop.md#classes).

> **Note for Object Oriented Programming users**:
>
> Python is strongly object-oriented in the sense that almost everything in the language is an object: numbers, strings, functions, etc.

We will now see how to use variables to store some literal constants, the simple integer and string data types.

## How to write Python programs

Henceforth, the standard procedure to save and run a Python program is as follows:

1. Open your editor of choice.
2. Type the program code given in the example.
3. Save it as a file with the filename mentioned.
4. Run the interpreter with the command `python program.py` to run the program.

### Example: Using Variables And Literal Constants

Type and run the following program:

```python
# Filename : var.py
i = 5
print(i)
i = i + 1
print(i)

s = '''This is a multi-line string.
This is the second line.'''
print(s)
```

Output:

```
5
6
This is a multi-line string.
This is the second line.
```

**How It Works**

Here's how this program works. First, we assign the literal constant value `5` to the variable `i` using the assignment operator (`=`). This line is called a statement because it states that something should be done and in this case, we connect the variable name `i` to the value `5`. Next, we print the value of `i` using the `print` statement which, unsurprisingly, just prints the value of the variable to the screen.

Then we add `1` to the value stored in `i` and store it back. We then print it and as expected, we get the value `6`.

Similarly, we assign the literal string to the variable `s` and then print it.

> **Note for static language programmers**
>
> Variables are used by just assigning them a value. No declaration or data type definition is needed/used.

## Logical and Physical Lines

A physical line is what you _see_ when you write the program. A logical line is what _Python sees_ as a single statement. Python implicitly assumes that each _physical line_ corresponds to a _logical line_.

An example of a logical line is a statement like `print 'hello world'` - if this was on a line by itself (as you see it in an editor), then this also corresponds to a physical line.

Most Python programmers encourage the use of a single statement per line which makes code more readable.

When a logical line has a starting parenthesis, square bracket, or curly brace but not an ending one, you may continue the expression on multiple lines until the matching parenthesis/bracket/brace is given on a later line. This is called *implicit line joining*. You can see this in action when we write programs using [lists](./11_data_structures.md#lists) in later chapters.

## Indentation

Whitespace is important in Python. Actually, *whitespace at the beginning of the line is important*. This is called _indentation_. Leading whitespace (spaces and tabs) at the beginning of the logical line is used to determine the indentation level of the logical line, which in turn is used to determine the grouping of statements.

This means that statements which go together _must_ have the same indentation. Each such set of statements is called a *block*. We will see examples of how blocks are important in later chapters.

One thing you should remember is that wrong indentation can cause errors. For example:

```python
i = 5
# Error below! Notice a single space at the start of the line
 print('Value is', i)
print('I repeat, the value is', i)
```

When you run this, you get the following error:

```
  File "whitespace.py", line 3
    print('Value is', i)
    ^
IndentationError: unexpected indent
```

Notice that there is a single space at the beginning of the second line. The error indicated by Python tells us that the syntax of the program is invalid i.e. the program was not properly written. We will learn more about blocks and indentation in later chapters such as the [control flow](./08_control_flow.md#control_flow) chapter.

> **How to indent**
>
> Use four spaces for indentation. This is the official Python language recommendation. Good editors will automatically do this for you. Make sure you use a consistent number of spaces for indentation, otherwise your program will not run or will have unexpected behavior. Make sure not to mix tabs and spaces.

## Summary

Now that we have learned some basics about strings, numbers, and Python's syntax, let's move on to more interesting uses of these basics, such as control flow statements. Be sure to become comfortable with what you have read in this chapter.

