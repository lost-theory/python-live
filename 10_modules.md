# Modules

You have seen how you can reuse code in your program by defining functions once. What if you wanted to reuse a number of functions in other programs that you write? The answer is modules.

The simplest way to write a module is to create a file with a `.py` extension that contains functions and variables. If you've followed along with running the example code, you've already created many modules without knowing it!

A module can be *imported* by another program to make use of its functionality. This is how we can use the Python standard library as well. First, we will see how to use the standard library modules.

Example (save as `module_using_sys.py`):

```python
import sys

print('The command line arguments are:')
print(sys.argv)
```

Output:

```
$ python module_using_sys.py
The command line arguments are:
['module_using_sys.py']

$ python module_using_sys.py --test=1 wow
The command line arguments are:
['module_using_sys.py', '--test=1', 'wow']

$ python module_using_sys.py 1 2 3 4 5 6 7 8 9 10
The command line arguments are:
['module_using_sys.py', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10']
```

**How It Works**

First, we *import* the `sys` module using the `import` statement. Basically, this translates to us telling Python that we want to use this module. The `sys` module contains functionality related to the Python interpreter and its environment i.e. the **sys**tem.

When Python executes the `import sys` statement, it looks for the `sys` module. In this case, it is one of the built-in modules, and hence Python knows where to find it.

If it was not a compiled module i.e. a module written in Python, then the Python interpreter will search for it in the directories listed in its `sys.path` variable. If the module is found, then the statements in the body of that module are run and the module is made *available* for you to use. Note that the initialization is done only the *first* time that we import a module.

The `argv` variable inside the `sys` module is accessed using the dotted notation i.e. `sys.argv`. It clearly indicates that this name is part of the `sys` module. Another advantage of this approach is that the name does not clash with any `argv` variable used in your program.

The `sys.argv` variable is a *list* of strings. Specifically, the `sys.argv` contains the list of *command line arguments* i.e. the arguments passed to your program using the command line.

Here, when we execute `python module_using_sys.py 1 2 3 4`, we run the module `module_using_sys.py` with the `python` command and the other things that follow are arguments passed to the program. Python stores the command line arguments in the `sys.argv` variable for us to use.

The name of the script running is always the first argument in the `sys.argv` list. So, in this case we will have `'module_using_sys.py'` as `sys.argv[0]`.

## The from..import statement

If you want to directly import the `argv` variable into your program (to avoid typing the `sys.` everytime for it), then you can use the `from sys import argv` statement.

Example:

```python
from math import sqrt
print("Square root of 16 is {}".format(sqrt(16)))
```

## Making Your Own Modules

Creating your own modules is easy, you've been doing it all along!  This is because every Python program is also a module. You just have to make sure it has a `.py` extension. The following example should make it clear.

Example (save as `mymodule.py`):

```python
def say_hi():
    print('Hi, this is mymodule speaking.')
```

The above was a sample *module*. As you can see, there is nothing particularly special about it compared to our usual Python program. We will next see how to use this module in our other Python programs.

Now we write another module which imports the first (save as `demo.py`):

```python
import mymodule
print(dir(mymodule))
mymodule.say_hi()
```

Output:

```
$ python demo.py
['__builtins__', '__doc__', '__file__', '__name__', '__package__', 'say_hi']
Hi, this is mymodule speaking.
```

**How It Works**

Notice that we use the same dotted notation to access members of the module. Python makes good reuse of the same notation to give the distinctive 'Pythonic' feel to it so that we don't have to keep learning new ways to do things.

Here is a version utilising the `from..import` syntax:

```python
from mymodule import say_hi
say_hi()
```

The output should be the same as the first `demo.py`.

## The `dir` function

You can use the built-in `dir` function to list the identifiers that an object defines. For example, for a module, the identifiers include the functions, classes and variables defined in that module.

When you supply a module name to the`dir()` function, it returns the list of the names defined in that module. When no argument is applied to it, it returns the list of names defined in the current module.

Example:

```bash
$ python
>>> import sys
>>> #get names of attributes in sys module, only a few entries are shown here...
>>> dir(sys)
['__displayhook__', '__doc__', 'argv', 'builtin_module_names', 'version', 'version_info', ...]

>>> #get names of attributes for current module
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__']

>>> # create a new variable 'a'
>>> a = 5
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__', 'a']

# delete 'a'
>>> del a
>>> dir()
['__builtins__', '__doc__', '__name__', '__package__']
```

**How It Works**

First, we see the usage of `dir` on the imported `sys` module. We can see the huge list of attributes that it contains.

Next, we use the `dir` function without passing parameters to it. By default, it returns the list of attributes for the current module. Notice that the list of imported modules is also part of this list.

In order to observe the `dir` in action, we define a new variable `a` and assign it a value and then check `dir` and we observe that there is an additional value in the list of the same name. We remove the variable/attribute of the current module using the `del` statement and the change is reflected again in the output of the `dir` function.

A note on `del` - this statement is used to *delete* a variable/name and after the statement has run, in this case `del a`, you can no longer access the variable `a` - it is as if it never existed before at all.

Note that the `dir()` function works on *any* object. For example, run `dir(str)` for the attributes of the `str` (string) class.

## Summary

Just like functions let you create reusable parts of programs, modules let you create reusable programs. We have seen how to use these modules and create our own modules and examine their contents using `dir`.
