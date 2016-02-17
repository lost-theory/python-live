# Data Structures

Data structures are basically just that - they are *structures* which can hold some *data* together. In other words, they are used to store a collection of related data.

The three main collection data structures in Python are _list, tuple, and dictionary_. We will see how to use each of them and how they make life easier for us.

## Lists

A `list` is a data structure that holds an ordered collection of items i.e. you can store a *sequence* of items in a list. This is easy to imagine if you can think of a shopping list where you have a list of items to buy, except that you probably have each item on a separate line in your shopping list whereas in Python you put commas in between them.

Lists of items are enclosed in square brackets, e.g. `[1, 2, 3]`. Once you have created a list, you can add, remove, and search for items in the list. Since we can add and remove items, we say that a list is a *mutable* data type i.e. this type can be altered.

All objects in Python, including data types like `list`, contain methods that allow you to perform some operation on that object. For example, to add an item to a list we use the `list.append(...)` method. Methods are looked up on an object using the `.` and called using parentheses just like a function. To see a list of methods on a given object, use `dir(...)` on that object.

Example (save as `ds_using_list.py`):

```python
# This is my shopping list
shoplist = ['apple', 'mango', 'carrot', 'banana']

print('I have', len(shoplist), 'items to purchase.')

print('These items are:', end=' ')
for item in shoplist:
    print(item, end=' ')

print('\nI also have to buy rice.')
shoplist.append('rice')
print('My shopping list is now', shoplist)

print('I will sort my list now.')
shoplist.sort()
print('Sorted shopping list is', shoplist)

print('The first item I will buy is', shoplist[0])
olditem = shoplist[0]
del shoplist[0]
print('I bought the', olditem)
print('My shopping list is now', shoplist)
```

Output:

```
$ python ds_using_list.py
I have 4 items to purchase.
These items are: apple mango carrot banana
I also have to buy rice.
My shopping list is now ['apple', 'mango', 'carrot', 'banana', 'rice']
I will sort my list now.
Sorted shopping list is ['apple', 'banana', 'carrot', 'mango', 'rice']
The first item I will buy is apple
I bought the apple
My shopping list is now ['banana', 'carrot', 'mango', 'rice']
```

**How It Works**

The variable `shoplist` is a shopping list for someone who is going to the market. In `shoplist`, we only store strings of the names of the items to buy but you can add _any kind of object_ to a list including numbers and even other lists.

We have also used the `for..in` loop to iterate through the items of the list. By now, you must have realised that a list is also a sequence. The speciality of sequences will be discussed in a [later section](#sequence).

Notice the use of the `end` parameter in the call to `print` function to indicate that we want to end the output with a space instead of the usual line break.

Next, we add an item to the list using the `append` method of the list object, as already discussed before. Then, we check that the item has been indeed added to the list by printing the contents of the list by simply passing the list to the `print` function which prints it neatly.

Then, we sort the list by using the `sort` method of the list. It is important to understand that this method affects the list itself and does not return a modified list - this is different from the way strings work. This is what we mean by saying that lists are _mutable_ and that strings are _immutable_.

Next, when we finish buying an item in the market, we want to remove it from the list. We achieve this by using the `del` statement. Here, we mention which item of the list we want to remove and the `del` statement removes it from the list for us.  We specify that we want to remove the first item from the list and hence we use `del shoplist[0]` (remember that Python starts counting from 0).

If you want to know all the methods defined by the list object, see `help(list)` for details.

## Tuples

Tuples are used to hold together multiple objects. They are similar to lists, but don't contain the extensive functionality that the lists give you. One major feature of tuples is that they are *immutable* like strings i.e. you cannot modify tuples

Tuples are defined by specifying items separated by commas within a pair of parentheses, e.g. `(1, 2)`.

Tuples are typically used in cases where you have a collection of items with a fixed size, the values inside the tuple will not change, and position of the items is important. For example, you could use a tuple to represent `(x, y)` coordinates on a map. There will always be two elements in the tuple, the first element is always the `x` value, and the second element is always the `y` value.

Example (save as `ds_using_tuple.py`):

```python
coords = (1, 8)
new_coords = (2, 7)
line_segment = (coords, new_coords)
print("The coordinates have {} values".format(len(coords)))
print("The first set of coordinates is {}".format(coords))
print("The second set of coordinates is {}".format(new_coords))
print("The set of X values are: {} and {}".format(coords[0], new_coords[0]))
print("The set of Y values are: {} and {}".format(coords[1], new_coords[1]))
print("There is a line segment between the two points: {}".format(line_segment))
print("The X value of the first point in the segment is: {}".format(line_segment[0][0]))
```

Output:

```
$ python ds_using_tuple.py
The coordinates have 2 values
The first set of coordinates is (1, 8)
The second set of coordinates is (2, 7)
The set of X values are: 1 and 2
The set of Y values are: 8 and 7
There is a line segment between the two points: ((1, 8), (2, 7))
The X value of the first point in the segment is: 1
```

**How It Works**

The variables `coords` and `new_coords` refer to tuples of `(x,y)` coordinates. We see that the `len` function can be used to get the length of the tuple. This also indicates that a tuple is a [sequence](#sequence) as well.

We use indexing with square brackets to look at the `x` and `y` values individually.

We can nest the tuples inside of another tuple, `line_segment`, which represents a line between the two points. Again, we can use indexing to grab the first item in the `line_segment` tuple `(1, 8)`, then look up the first element of that tuple to get the integer `1`.

> **Tuples with 0 or 1 items**
>
> An empty tuple is constructed by an empty pair of parentheses such as `myempty = ()`. However, a tuple with a single item is not so simple. You have to specify it using a comma following the first (and only) item so that Python can differentiate between a tuple and a pair of parentheses surrounding the object in an expression i.e. you have to specify `singleton = (2,)` if you mean you want a tuple containing the single item `2`.

## Dictionaries

A dictionary is like an address-book where you can find the address of a person by knowing only the person's name. It allows us to associate *keys* with *values*. Note that the keys in a dictionary must all be unique, e.g. if two people have the exact same name, you have no way of differentiating which phone number belongs to which name.

Note that you can use only immutable objects (like strings and tuples) for the keys of a dictionary, but you can use any object for the values of the dictionary. This means that keys are typically simpler objects, while values can be more complex.

Pairs of keys and values are specified in a dictionary by using the notation `d = {key1: value1, key2: value2}`. Notice that the key-value pairs are separated by a colon and the pairs are separated themselves by commas, and all this is enclosed in a pair of curly braces.

Key-value pairs in a dictionary are not ordered in any manner. If you want a particular order, then you will have to sort them yourself before using it.

Dictionaries in Python are represented by the `dict` class.

Example (save as `ds_using_dict.py`):

```python
book = {
    'Swaroop': 'swaroop@swaroopch.com',
    'Larry': 'larry@wall.org',
    'Matsumoto': 'matz@ruby-lang.org',
    'Spammer': 'spammer@hotmail.com'
}

print("Swaroop's address is", book['Swaroop'])

# Deleting a key-value pair
del book['Spammer']

print('There are {} contacts in the address book.'.format(len(book)))

for name, address in book.items():
    print('Contact {} at {}'.format(name, address))

# Adding a key-value pair
book['Guido'] = 'guido@python.org'

if 'Guido' in book:
    print("Guido's address is", book['Guido'])
```

Output:

```
$ python ds_using_dict.py
Swaroop's address is swaroop@swaroopch.com
There are 3 contacts in the address book.
Contact Swaroop at swaroop@swaroopch.com
Contact Matsumoto at matz@ruby-lang.org
Contact Larry at larry@wall.org
Guido's address is guido@python.org
```

**How It Works**

We create the dictionary `book` using the notation already discussed. We then access key-value pairs by specifying the key using the indexing operator as discussed in the context of lists and tuples. Observe the simple syntax.

We can delete key-value pairs using the `del` statement. We simply specify the dictionary and the indexing operator for the key to be removed and pass it to the `del` statement. There is no need to know the value corresponding to the key for this operation.

Next, we access each key-value pair of the dictionary using the `items` method of the dictionary which returns a list of tuples where each tuple contains a pair of items - the key followed by the value. We retrieve this pair and assign it to the variables `name` and `address` correspondingly for each pair using the `for..in` loop and then print these values in the for-block.

We can add new key-value pairs by simply using the indexing operator to access a key and assign that value, as we have done for Guido in the above case.

We can check if a key-value pair exists using the `in` operator.

For the list of methods of the `dict` class, see `help(dict)`.

## Sequences

Lists, tuples, dicts, and strings are examples of "sequences" or "containers", but what are sequences and what is so special about them?

The major features are *membership tests*, (i.e. the `in` and `not in` expressions) and *indexing operations*, which allow us to fetch a particular item in the sequence directly. We can also `iterate` over them using the `for` loop.

Three of the sequence types mentioned above, lists, tuples, and strings, also have a *slicing* operation which allows us to retrieve a range of indices at once from the sequence. Dictionaries do not have the same slicing operation because they are unordered.

Example (save as `ds_seq.py`):

```python
vegetable = 'carrot'
shoplist = ['apple', 'mango', 'banana', vegetable]

# Indexing or 'subscript' operations
print('Item 0 is', shoplist[0])
print('Item 1 is', shoplist[1])
print('Item 2 is', shoplist[2])
print('Item 3 is', shoplist[3])
print('Item -1 is', shoplist[-1])
print('Item -2 is', shoplist[-2])
print('Character 0 of the vegetable is', vegetable[0])

# Slicing a list
print('Item 1 to 3 is', shoplist[1:3])
print('Item 2 to end is', shoplist[2:])
print('Item 1 to -1 is', shoplist[1:-1])
print('Item start to end is', shoplist[:])

# Slicing a string
print('characters 1 to 3 is', vegetable[1:3])
print('characters 2 to end is', vegetable[2:])
print('characters 1 to -1 is', vegetable[1:-1])
print('characters start to end is', vegetable[:])
```


Output:

```
Item 0 is apple
Item 1 is mango
Item 2 is banana
Item 3 is carrot
Item -1 is carrot
Item -2 is banana
Character 0 of the vegetable is c
Item 1 to 3 is ['mango', 'banana']
Item 2 to end is ['banana', 'carrot']
Item 1 to -1 is ['mango', 'banana']
Item start to end is ['apple', 'mango', 'banana', 'carrot']
characters 1 to 3 is ar
characters 2 to end is rrot
characters 1 to -1 is arro
characters start to end is carrot
```

**How It Works**

First, we see how to use indexes to get individual items of a sequence. This is also referred to as the _subscription operation_. Whenever you specify a number to a sequence within square brackets as shown above, Python will fetch you the item corresponding to that position in the sequence. Remember that Python starts counting numbers from 0. Hence, `shoplist[0]` fetches the first item and `shoplist[3]` fetches the fourth item in the `shoplist`sequence.

The index can also be a negative number, in which case, the position is calculated from the end of the sequence. Therefore, `shoplist[-1]` refers to the last item in the sequence and `shoplist[-2]` fetches the second last item in the sequence.

The slicing operation is used by specifying the name of the sequence followed by an optional pair of numbers separated by a colon within square brackets. Note that this is very similar to the indexing operation you have been using till now. Remember the numbers are optional but the colon isn't.

The first number (before the colon) in the slicing operation refers to the position from where the slice starts and the second number (after the colon) indicates where the slice will stop at. If the first number is not specified, Python will start at the beginning of the sequence. If the second number is left out, Python will stop at the end of the sequence. Note that the slice returned _starts_ at the start position and will end just before the _end_ position i.e. the start position is included but the end position is excluded from the sequence slice.

Thus, `shoplist[1:3]` returns a slice of the sequence starting at position 1, includes position 2 but stops at position 3 and therefore a *slice* of two items is returned.  Similarly, `shoplist[:]` returns a copy of the whole sequence.

You can also do slicing with negative positions. Negative numbers are used for positions from the end of the sequence. For example, `shoplist[:-1]` will return a slice of the sequence which excludes the last item of the sequence but contains everything else.

You can also provide a third argument for the slice, which is the _step_ for the slicing (by default, the step size is 1):

```python
>>> shoplist = ['apple', 'mango', 'carrot', 'banana']
>>> shoplist[::1]
['apple', 'mango', 'carrot', 'banana']
>>> shoplist[::2]
['apple', 'carrot']
>>> shoplist[::3]
['apple', 'banana']
>>> shoplist[::-1]
['banana', 'carrot', 'mango', 'apple']
```

Notice that when the step is 2, we get the items with position 0, 2,... When the step size is 3, we get the items with position 0, 3, etc.

Try various combinations of indexing and slicing using the Python interpreter interactively so that you can see the results immediately. The great thing about sequences is that you can access tuples, lists, and strings all in the same way!

## References

When you create an object and assign it to a variable, the variable only _refers_ to the object and does not represent the object itself!  That is, the variable name points to that part of your computer's memory where the object is stored. This is called *binding* the name to the object.

Generally, you don't need to be worried about this, but there is a subtle effect due to references which you need to be aware of:

Example (save as `ds_reference.py`):

```python
# mylist is just another name pointing to the same object!
shoplist = ['apple', 'mango', 'carrot', 'banana']
mylist = shoplist

# I purchased the first item, so I remove it from the list
del shoplist[0]

# Notice that both shoplist and mylist both print
# the same list without the 'apple' confirming that
# they point to the same object
print('shoplist is', shoplist)
print('mylist is', mylist)

# Copy the list using shoplist[:], a slice that covers the entire list
mylist = shoplist[:]

# Now what happens if we remove the first item?
del mylist[0]
print('shoplist is', shoplist)
print('mylist is', mylist)
# Notice that now the two lists are different
```

Output:

```
shoplist is ['mango', 'carrot', 'banana']
mylist is ['mango', 'carrot', 'banana']
shoplist is ['mango', 'carrot', 'banana']
mylist is ['carrot', 'banana']
```

**How It Works**

Most of the explanation is available in the comments.

Remember that if you want to make a copy of a list or such kinds of sequences or complex objects (not simple _objects_ such as integers), then you have to use the slicing operation to make a copy. If you just assign the variable name to another name, both of them will "refer" to the same object and this could be trouble if you are not careful.

## More About Strings

We have already discussed strings in detail earlier. What more can there be to know?  Well, did you know that strings are also objects and have methods for doing all kinds of useful operations?

The strings that you use in program are all objects of the class `str`. Some useful methods of this class are demonstrated in the next example. For a complete list of such methods, see `dir(str)` or `help(str)`.

Example (save as `ds_str_methods.py`):

```python
name = 'Swaroop'

if name.startswith('Swa'):
    print('Yes, the string starts with "Swa"')

if 'a' in name:
    print('Yes, it contains the string "a"')

if name.find('war') != -1:
    print('Yes, it contains the string "war"')

delimiter = ' | '
mylist = ['Brazil', 'Russia', 'India', 'China']
print(delimiter.join(mylist))
```

Output:

```
$ python ds_str_methods.py
Yes, the string starts with "Swa"
Yes, it contains the string "a"
Yes, it contains the string "war"
Brazil | Russia | India | China
```

**How It Works**

Here, we see a few of the string methods in action. The `startswith` method is used to find out whether the string starts with the given string. The `in` operator is used to check if a given string is a part of the string.

The `find` method is used to locate the position of the given substring within the string. `find` returns -1 if it is unsuccessful in finding the substring. The `str` class also has a neat method to `join` the items of a sequence with the string acting as a delimiter between each item of the sequence and returns a bigger string generated from this.

## Summary

We have explored the various built-in data structures of Python in detail. They are essential for doing useful things with real world data.
