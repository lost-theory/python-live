# Learn Python Live

This is the repository for Learn Python Live, an accelerated, interactive class teaching Python.

The first half of this material comes from "A Byte of Python", an open source book that teaches programming using the Python language. It was created by [Swaroop C H](http://www.swaroopch.com/) and is available [here](http://python.swaroopch.com/). All credit goes to Swaroop for creating such a great resource for learning Python. The original "A Byte of Python" material has been forked for use in a class setting. See the bottom section for a list of changes.

The second half of the material is new and walks you through more functionality in the standard library, installing open source Python libraries, and writing a simple web application with Flask.

## Chapters

Byte of Python chapters:

* BoP Ch. 3: [About Python](03_about_python.md) (skip this if you want to start coding right away)
* BoP Ch. 4: [Installation](04_installation.md) (skip if you are using Koding.com)
* BoP Ch. 5: [First Steps](05_first_steps.md)
* BoP Ch. 6: [Basics](06_basics.md)
* BoP Ch. 7: [Operators and Expressions](07_op_exp.md)
* BoP Ch. 8: [Control Flow](08_control_flow.md)
* BoP Ch. 9: [Functions](09_functions.md)
* BoP Ch. 11: [Data Structures](11_data_structures.md)
* BoP Ch. 10: [Modules](10_modules.md)
* BoP Ch. 14: [Input and Output](14_io.md)
* BoP Ch. 13: [Object Oriented Programming](13_oop.md)
* BoP Ch. 15: [Exceptions](15_exceptions.md)

To see the omitted Byte of Python chapters, see the [original book](http://python.swaroopch.com/).

New material created for Learn Python Live:

* LPL Ch. 1: [Advanced Python features](lpl_01_advanced_python_features.md)
* LPL Ch. 2: [Useful modules in the standard library](lpl_02_useful_stdlib_modules.md)
* LPL Ch. 3: [Making HTTP requests and using JSON](lpl_03_http_requests_and_json.md)
* LPL Ch. 4: [Installing open source libraries](lpl_04_installing_new_modules.md)
* (Flask chapters coming soon)

## License

This book is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

This means:

- You are free to share i.e. to copy, distribute and transmit this book
- You are free to remix i.e. to make changes to this book
- You are free to use it for commercial purposes

Please note:

- Please do *not* sell electronic or printed copies of the book unless you have clearly and prominently mentioned in the description that these copies are *not* from the original author of this book.
- Attribution *must* be shown in the introductory description and front page of the document by linking back to http://python.swaroopch.com/ and clearly indicating that the original text can be fetched from this location.
- All the code/scripts provided in this book is licensed under the [3-clause BSD License](http://www.opensource.org/licenses/bsd-license.php) unless otherwise noted.

## Changes from "A Byte of Python"

* Python 3 -> Python 2 (though I kept some of the parentheses on `print` statements where it doesn't affect the output).
* Cut some beginning chapters and appendices to give us just the essential information about learning the language.
* Removed files related to the publishing of the book (book.json, etc.).
* Put Ch. 11 (Data Structures) ahead of Ch. 10 (Modules) so we can write a program using simple values, conditions, loops, functions, and lists/dicts without needing to learn about modules yet. Ch. 10 (Modules) will then be our stepping stone to the standard library and writing larger programs.
* Put Ch. 14 (IO using files) ahead of Ch. 13 (OOP) so we can teach files first and then teach OOP on its own as a larger subject.
* Some small edits and paring down of chapter content to make it easier to teach interactively.
* Removed and rewrote example code (original BoP code is available [here](https://github.com/swaroopch/byte-of-python/tree/master/programs)). Example code is now embedded directly into the chapters to make it easier to browse the book directly on Github.
