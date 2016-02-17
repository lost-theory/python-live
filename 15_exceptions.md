# Exceptions

Exceptions occur when _exceptional_ situations occur in your program. For example, what if you are going to read a file and the file does not exist? Or what if you accidentally deleted it when the program was running? Such situations are handled using **exceptions**.

Similarly, what if your program had some invalid statements? This is handled by Python which **raises** its hands and tells you there is an **error**.

## Errors

Consider a simple `print` function call. What if we misspelt `print` as `Print`? Note the capitalization. In this case, Python _raises_ a syntax error.

```python
>>> Print("Hello World")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'Print' is not defined
>>> print("Hello World")
Hello World
```

Observe that a `NameError` is raised and also the location where the error was detected is printed. This is what an **error handler** for this error does.

## Exceptions

We will **try** to read input from the user. Press `[ctrl-d]` and see what happens.

```python
>>> s = raw_input('Enter something: ')
Enter something: Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
EOFError
```

Python raises an error called `EOFError` which basically means it found an *end of file* symbol (which is represented by `ctrl-d`) when it did not expect to see it.

## Handling Exceptions

We can handle exceptions using the `try..except` statement.  We basically put our usual statements within the try-block and put all our error handlers in the except-block.

## Raising Exceptions

You can _raise_ exceptions using the `raise` statement by providing the name of the error/exception and the exception object that is to be _thrown_.

The error or exception that you can raise should be a class which directly or indirectly must be a derived class of the `Exception` class.

## The `with` statement

Acquiring a resource in the `try` block and subsequently releasing the resource in the `finally` block is a common pattern. The `with` statement was added to Python to automatically handle this pattern. The built-in `file` object can be used in the `with` statement to safely close the file even in the case of errors, without having to write your own error handling code.

## Summary

We have discussed the usage of the `try`, `except`, and `raise`. We have seen how to create our own exception types and how to raise exceptions as well. We also briefly saw how the `with` statement can be used to handle errors when accessing file objects.
