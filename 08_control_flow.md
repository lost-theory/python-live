# Control Flow

In the programs we have seen till now, there has always been a series of statements faithfully executed by Python in exact top-down order. What if you wanted to change the flow of how it works? For example, you want the program to take some decisions and do different things depending on different situations, such as printing 'Good Morning' or 'Good Evening' depending on the time of the day? Or repeating some part of the program until something happens?

As you might have guessed, this is achieved using control flow statements. There are three control flow statements in Python - `if`, `for` and `while`.

## The `if` statement

The `if` statement is used to check a condition: *if* the condition is true, we run a block of statements (called the _if-block_), *else* we process another block of statements (called the _else-block_). The *else* clause is optional.

```python
number = 23
guess = int(raw_input('Enter an integer: '))

if guess == number:
    print('Congratulations, you guessed it!')
elif guess < number:
    print('No, it is a little higher than that')
else:
    # this is the `else` clause, you must have guessed > number to reach here
    print('No, it is a little lower than that')

print('Done')
```

Output:

```
$ python if.py
Enter an integer: 50
No, it is a little lower than that
Done

$ python if.py
Enter an integer: 22
No, it is a little higher than that
Done

$ python if.py
Enter an integer: 23
Congratulations, you guessed it!
Done
```

**How It Works**

In this program, we take guesses from the user and check if it is the number that we have. We set the variable `number` to any integer we want, say `23`. Then, we take the user's guess using the `raw_input()` function. Functions are just reusable pieces of programs. We'll read more about them in the [next chapter](./09_functions.md#functions).

We supply a string to the built-in `input` function which prints it to the screen and waits for input from the user. Once we enter something and press the `[enter]` key, the `raw_input()` function returns what we entered, as a string. We then convert this string to an integer using `int` and then store it in the variable `guess`. Actually, the `int` is a class but all you need to know right now is that you can use it to convert a string to an integer (assuming the string contains a valid integer in the text).

Next, we compare the guess of the user with the number we have chosen. If they are equal, we print a success message. Notice that we use indentation levels to tell Python which statements belong to which block. This is why indentation is so important in Python. I hope you are sticking to the "consistent indentation" rule. Are you?

Notice how the `if` statement contains a colon `:` at the end, this is used to indicate to Python that an indented block of statements follows.

Then, we check if the guess is less than the number, and if so, we inform the user that they must guess a little higher than that. What we have used here is the `elif` clause which actually combines two related `if else-if else` statements into one combined `if-elif-else` statement. This makes the program easier and reduces the amount of indentation required.

The `elif` and `else` statements must also have a colon at the end of the logical line followed by their corresponding block of statements (with proper indentation, of course)

You can have another `if` statement inside the if-block of an `if` statement and so on - this is called a nested `if` statement.

Remember that the `elif` and `else` parts are optional. A minimal valid `if` statement is:

```python
if True:
    print('Yes, it is true')
```

After Python has finished executing the complete `if` statement along with the associated `elif` and `else` clauses, it moves on to the next statement in the block containing the `if` statement. In this case, it is the main block (where execution of the program starts), and the next statement is the `print('Done')` statement. After this, Python sees the ends of the program and simply finishes up.

> **Note for C/C++ Programmers**
>
> There is no `switch` statement in Python. You can use an `if..elif..else` statement to do the same thing (and in some cases, use a [dictionary](./11_data_structures.md#dictionary) to do it quickly)

## The while Statement

The `while` statement allows you to repeatedly execute a block of statements as long as a condition is true. A `while` statement is an example of what is called a *looping* statement. A `while` statement can have an optional `else` clause.

Example (save as `while.py`):

```python
number = 23
running = True

while running:
    guess = int(input('Enter an integer: '))

    if guess == number:
        print('Congratulations, you guessed it.')
        running = False # this causes the while loop to stop
    elif guess < number:
        print('No, it is a little higher than that.')
    else:
        print('No, it is a little lower than that.')

print('Done')
```

Output:

```
$ python while.py
Enter an integer: 50
No, it is a little lower than that.
Enter an integer: 22
No, it is a little higher than that.
Enter an integer: 23
Congratulations, you guessed it.
Done
```

**How It Works**

In this program, we are still playing the guessing game, but the advantage is that the user is allowed to keep guessing until he guesses correctly - there is no need to repeatedly run the program for each guess, as we have done in the previous section. This aptly demonstrates the use of the `while` statement.

We move the `input` and `if` statements to inside the `while` loop and set the variable `running` to `True` before the while loop. First, we check if the variable `running` is `True` and then proceed to execute the corresponding *while-block*. After this block is executed, the condition is again checked which in this case is the `running` variable. If it is true, we execute the while-block again.

## The `for` loop

The `for..in` statement is another looping statement which *iterates* over a sequence of objects i.e. go through each item in a sequence. We will see more about [sequences](./11_data_structures.md#sequence) in detail in later chapters. What you need to know right now is that a sequence is just an ordered collection of items, like integers or strings.

Example (save as `for.py`):

```python
for i in range(5):
    print(i)
print('The for loop is over')
```

Output:

```
$ python for.py
0
1
2
3
4
The for loop is over
```

**How It Works**

In this program, we are printing a *sequence* of numbers. We generate this sequence of numbers using the built-in `range` function.

What we do here is supply `range` with a number that will be counted up to, starting at 0. For example, `range(5)`, will start at 0 and count by 1 up to (but **not including** 5), giving `[0, 1, 2, 3, 4]`.

Note that `range()` generates a list of numbers all at once for us to use. Lists are explained in the [data structures chapter](./11_data_structures.md#data-structures).

The `for` loop then iterates over this range - `for i in range(5)` is equivalent to `for i in [0, 1, 2, 3, 4]`. The `i` variable is assigned to each value in the sequence, one at a time, and then the block of statements in the body of the loop is executed.  In this case, we just print the value in the block of statements.

Remember that the `for..in` loop works for any sequence. Here, we have a list of numbers generated by the built-in `range` function, but in general we can use any kind of sequence of any kind of object! We will explore this idea in detail in later chapters.

> **Note for C/C++/Java/C# Programmers**
> 
> The Python `for` loop is radically different from the C/C++ `for` loop. C# programmers will note that the `for` loop in Python is similar to the `foreach` loop in C#. Java programmers will note that the same is similar to `for (int i : IntArray)` in Java 1.5.
> 
> In C/C++, if you want to write `for (int i = 0; i < 5; i++)`, then in Python you write just `for i in range(5)`. As you can see, the `for` loop is simpler, more expressive and less error prone in Python.

## The `break` and `continue` Statements

While inside a loop body, you can use `break` to immediately exit the loop, and `continue` to skip over the current iteration to the next iteration. [Official docs](https://docs.python.org/2/tutorial/controlflow.html#break-and-continue-statements-and-else-clauses-on-loops). [A Byte of Python's section on break](http://python.swaroopch.com/control_flow.html#break-statement) and [continue](http://python.swaroopch.com/control_flow.html#continue-statement).

## Summary

We have seen how to use the three control flow statements - `if`, `while` and `for` along with their associated `break` and `continue` statements. These are some of the most commonly used parts of Python and hence, becoming comfortable with them is essential.

Next, we will see how to create and use functions.
