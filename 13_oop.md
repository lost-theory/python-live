# Object Oriented Programming

In all the programs we wrote till now, we have designed our program around functions i.e. blocks of statements which manipulate data. This is called the _procedure-oriented_ way of programming. There is another way of organizing your program which is to combine data and functionality and wrap it inside something called an object. This is called the _object oriented_ programming paradigm. Most of the time you can use procedural programming, but when writing large programs or have a problem that is better suited to this method, you can use object oriented programming techniques.

Classes and objects are the two main aspects of object oriented programming. A **class** creates a new _type_ where **objects** are **instances** of the class. An analogy is that you can have variables of type `int` which translates to saying that variables that store integers are variables which are instances (objects) of the `int` class.

Objects can store data using ordinary variables that _belong_ to the object. Variables that belong to an object or class are referred to as **fields**. Objects can also have functionality by using functions that _belong_ to a class. Such functions are called **methods** of the class. This terminology is important because it helps us to differentiate between functions and variables which are independent and those which belong to a class or object. Collectively, the fields and methods can be referred to as the **attributes** of that class.

Fields are of two types - they can belong to each instance/object of the class or they can belong to the class itself. They are called **instance variables** and **class variables** respectively.

A class is created using the `class` keyword. The fields and methods of the class are listed in an indented block.

## The `self`

Class methods have only one specific difference from ordinary functions - they must have an extra first name that has to be added to the beginning of the parameter list, but you **do not** give a value for this parameter when you call the method, Python will provide it. This particular variable refers to the object _itself_, and by convention, it is given the name `self`.

Although, you can give any name for this parameter, it is _strongly recommended_ that you use the name `self` - any other name is definitely frowned upon. There are many advantages to using a standard name - any reader of your program will immediately recognize it and even specialized IDEs (Integrated Development Environments) can help you if you use `self`.

> **Note for C++/Java/C# Programmers**
>
> The `self` in Python is equivalent to the `this` pointer in C++ and the `this` reference in Java and C#.

You must be wondering how Python gives the value for `self` and why you don't need to give a value for it. An example will make this clear. Say you have a class called `MyClass` and an instance of this class called `myobject`. When you call a method of this object as `myobject.method(arg1, arg2)`, this is automatically converted by Python into `MyClass.method(myobject, arg1, arg2)` - this is all the special `self` is about.

This also means that if you have a method which takes no arguments, then you still have to have one argument - the `self`.

## Classes

The simplest class possible is shown in the following example (save as `oop_simplestclass.py`).

```python
class Person(object):
    pass # An empty block

p = Person()
print(p)
```

Output:

```bash
$ python oop_simplestclass.py
<__main__.Person instance at 0x10171f518>
```

**How It Works**

We create a new class using the `class` statement and the name of the class. This is followed by an indented block of statements which form the body of the class. In this case, we have an empty block which is indicated using the `pass` statement.

Next, we create an object/instance of this class using the name of the class followed by a pair of parentheses. (We will learn more about instantiation with `__init__` in the next section). For our verification, we confirm the type of the variable by simply printing it. It tells us that we have an instance of the `Person` class in the `__main__` module.

Notice that the address of the computer memory where your object is stored is also printed. The address will have a different value on your computer since Python can store the object wherever it finds space.

## Methods

We have already discussed that classes/objects can have methods just like functions except that we have an extra `self` variable. We will now see an example (save as `oop_method.py`).

```python
class Person(object):
    def greetings(self):
        print('Hello, how are you?')

p = Person()
p.greetings()

# You could also call `greetings` directly on the object returned by Person()
Person().greetings()
```

Output:

```bash
$ python oop_method.py
Hello, how are you?
Hello, how are you?
```

**How It Works**

Here we see the `self` in action. Notice that the `greetings` method takes no parameters but still has the `self` in the function definition.

## The `__init__` method

There are many method names which have special significance in Python classes. We will see the significance of the `__init__` method now.

The `__init__` method is run as soon as an object of a class is instantiated. The method is useful to do any *initialization* you want to do with your object. Notice the double underscores both at the beginning and at the end of the name.

Example (save as `oop_init.py`):

```python
class Person(object):
    def __init__(self, name):
        self.name = name

    def greetings(self):
        print('Hello, my name is', self.name)

p = Person('Swaroop')
p.greetings()
```

Output:

```bash
$ python oop_init.py
Hello, my name is Swaroop
```

**How It Works**

Here, we define the `__init__` method as taking a parameter `name` (along with the usual `self`). Here, we just create a new field also called `name`. Notice these are two different variables even though they are both called 'name'. There is no problem because the dotted notation `self.name` means that there is something called "name" that is part of the object called "self" and the other `name` is a local variable. Since we explicitly indicate which name we are referring to, there is no confusion.

Most importantly, notice that we do not explicitly call the `__init__` method but pass the arguments in the parentheses following the class name when creating a new instance of the class. This is the special significance of this method.

Now, we are able to use the `self.name` field in our methods which is demonstrated in the `greetings` method.

## Class And Object Variables

We have already discussed the functionality part of classes and objects (i.e. methods), now let us learn about the data part. The data part, i.e. fields, are nothing but ordinary variables that are _bound_ to the **namespaces** of the classes and objects. This means that these names are valid within the context of these classes and objects only. That's why they are called _name spaces_.

There are two types of _fields_ - class variables and object variables which are classified depending on whether the class or the object _owns_ the variables respectively.

**Class variables** are shared - they can be accessed by all instances of that class. There is only one copy of the class variable and when any one object makes a change to a class variable, that change will be seen by all the other instances.

**Object variables** are owned by each individual object/instance of the class. In this case, each object has its own copy of the field i.e. they are not shared and are not related in any way to the field by the same name in a different instance. An example will make this easy to understand (save as `oop_objvar.py`):

```python
class Robot(object):
    """Represents a robot with a name."""

    #A class variable
    greet_msgs = {'R2-D2': 'Beep boop boop bee!', 'C-3PO': 'Greetings, master.'}

    def __init__(self, name):
        #An instance variable
        self.name = name

    def greetings(self):
        message = Robot.greet_msgs.get(self.name, '...')
        print "{}: {}".format(self.name, message)

    @classmethod
    def available_robots(cls):
        """Returns which robots are available."""
        return cls.greet_msgs.keys()

droid1 = Robot("R2-D2")
droid2 = Robot("C-3PO")

droid1.greetings()
droid2.greetings()

# Calling the class method:
print "Available robots:", Robot.available_robots()

# Class methods can also be called on an instance:
print "Available robots:", droid1.available_robots()
print "Available robots:", droid2.available_robots()
```

Output:

```bash
$ python oop_objvar.py
R2-D2: Beep boop boop bee!
C-3PO: Greetings, master.
Available robots: ['R2-D2', 'C-3PO']
Available robots: ['R2-D2', 'C-3PO']
Available robots: ['R2-D2', 'C-3PO']
```

**How It Works**

This example shows the use of a class variable `greetings`, an instance variable `name`, and a classmethod `available_robots`.

Inside the `greetings` method we are using both the class variable and the instance variable. The class variable is referred to as `Robot.greetings`, while the instance variable is referred to as `self.name`.

We have marked the `available_robots` method as a class method using a "decorator", which we will learn in a later chapter. Decorators are basically a shortcut for adding a wrapper around a function. Applying the `@classmethod` decorator above a method `f` is the same as writing `f = classmethod(f)`.

This classmethod takes a single argument, `cls`, which is the `Robot` class object itself. We use `cls` to look up `cls.greetings`. We could have also written `Robot.greetings` here.

## Inheritance

One of the major benefits of object oriented programming is **reuse** of code and one of the ways this is achieved is through the **inheritance** mechanism. Inheritance can be best imagined as implementing a **type and subtype** relationship between classes.

Suppose you want to write a program which has to keep track of the teachers and students in a college. They have some common characteristics such as name, age, address, etc. They also have specific characteristics that may apply to one but not the other, such as salary, courses, grades, etc. They may also have different kinds of behavior, e.g. students can submit assignments, while teachers grade the assignments.

You can create two independent classes for each type and treat everything separately, but adding a new common characteristic would mean adding the code to both of these independent classes. This quickly becomes unwieldy.

A better way would be to create a common class to represent `Person` with all the common characteristics and behavior, and then have the `Teacher` and `Student` classes _inherit_ from this class to specialize it. The subclasses will become sub-types of the common type and add whatever specific characteristics and behavior are needed.

There are many advantages to this approach. If we add/change any functionality in `Person`, this is automatically reflected in the subtypes as well. For example, you can add a new ID card field for both teachers and students by simply adding it to the `Person` class. However, changes in the subtypes do not affect other subtypes. Another advantage is that if you can refer to a teacher or student object as a `Person` object which could be useful in some situations such as counting of the number of school members. This is called **polymorphism** where a sub-type can be substituted in any situation where a parent type is expected i.e. the object can be treated as an instance of the parent class.

Also observe that we reuse the code of the parent class and we do not need to repeat it in the different classes as we would have had to in case we had used independent classes.

The `Person` class in this situation is known as the **base class** or **superclass**. The `Teacher` and `Student` classes are called the **derived classes** or **subclasses**.

We will now see this example as a program (save as `oop_subclass.py`):

```python
class Person(object):
    def __init__(self, name):
        self.name = name

    def greetings(self):
        return "I am {}.".format(self.name)

    def __repr__(self):
        return '<Person name={!r}>'.format(self.name)

class Teacher(Person):
    def __init__(self, name, subject):
        Person.__init__(self, name)
        self.subject = subject
        self.students = []

    def greetings(self):
        message = Person.greetings(self)
        return "Greetings class. {} I will be your {} teacher this semester.".format(message, self.subject)

    def add_student(self, student):
        self.students.append(student)

    def grades(self):
        out = []
        for s in self.students:
            out.append([s.name, s.grade])
        return out

class Student(Person):
    def __init__(self, name, grade):
        Person.__init__(self, name)
        self.grade = grade

if __name__ == "__main__":
    t = Teacher('Mr. Garvey', 'Biology')
    print t.greetings()

    students = [
        Student('Jacqueline', "A"),
        Student('Blake', "C"),
        Student('Denice', "B+"),
        Student('Aaron', "B"),
    ]
    for s in students:
        print s.greetings()
        t.add_student(s)

    print "Current grades:"
    print t.grades()
```

Output:

```bash
$ python oop_objvar.py
Greetings class. I am Mr. Garvey. I will be your Biology teacher this semester.
I am Jacqueline.
I am Blake.
I am Denice.
I am Aaron.
Current grades:
[['Jacqueline', 'A'], ['Blake', 'C'], ['Denice', 'B+'], ['Aaron', 'B']]
```

**How It Works**

To use inheritance, we specify the base class names in a tuple following the class name in the class definition. Next, we observe that the `__init__` method of the base class is explicitly called using the `self` variable so that we can initialize the base class part of the object. This is very important to remember - Python does not automatically call the constructor of the base class, you have to explicitly call it yourself.

We also observe that we can call methods of the base class by prefixing the class name to the method call and then pass in the `self` variable along with any arguments.

Notice that we can treat instances of `Teacher` or `Student` as just instances of the `Person` classwhen we use the `greetings` method. The `greetings` method is common to all three classes, so it is safe to call from any of them.

Also, observe that the `greetings` method for `Teacher` extends the behavior of the `greetings` method in `Person`. One way to understand this is that Python _always_ starts looking for methods in the actual type, which in this case exists. If it could not find the method, it starts looking at the methods belonging to its base classes one by one in the order they are specified in the tuple in the class definition.

A note on terminology - if more than one class is listed in the inheritance tuple, then it is called **multiple inheritance**.

## Summary

We have now explored the various aspects of classes and objects as well as the various terminologies associated with it. We have also seen the benefits and pitfalls of object-oriented programming. Python is highly object-oriented and understanding these concepts carefully will help you a lot in the long run.
