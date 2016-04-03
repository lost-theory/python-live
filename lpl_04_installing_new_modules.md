## Installing open source modules

Python's built-in functionality and standard library is great, but there's an even larger library of modules in the open source community. Python is used in many fields: web apps, science, data analysis, machine learning, embedded programming, network programming, GUI / desktop applications, automation, etc. By installing new packages from Python's open source community you can tap into these fields and be way more productive than if you were building everything yourself from scratch.

To install packages, we are going to use two tools: `virtualenv` and `pip`. First let's make sure these two tools are installed, then we'll talk about what they're used for.

## Installing `virtualenv` and `pip`

First check if you already have `virtualenv` and `pip` installed by running the following commands:

```
$ pip --version
pip 6.1.1 from /usr/lib/python2.7/site-packages/pip-6.1.1-py2.7.egg (python 2.7)

$ virtualenv --version
12.1.1
```

If either of those complain that the command is not found, then you will need to install the tools that are missing:

* If you are using Ubuntu (the default for c9.io, Nitrous, and Koding), the command to use is: `sudo apt-get install -y python-virtualenv`. This will install both `pip` and `virtualenv`.
* If the `pip` command above succeeded, run `pip install virtualenv` to get Virtualenv.
* If the `pip` command failed, [download and run `get-pip.py`](https://pip.pypa.io/en/stable/installing/), then run `pip install virtualenv`.
* Another option is to install a newer version of Python. Python versions >=2.7.9 and >=3.4 both have `pip` installed by default. You can download the latest version of Python from the [official downloads page](https://www.python.org/downloads/).

If any of the above commands failed with "Permission denied", try running the same command with `sudo`. However, once `virtualenv` and `pip` are installed, you should no longer use `sudo` because these commands require no special permissions and work best when run as your local user.

Now let's learn what these commands do.

## Virtualenv: Create isolated Python environments

[Virtualenv](https://virtualenv.readthedocs.org/en/latest/) is a tool that gives you isolated or "virtual" Python environments that are independent of the system-wide Python installation.

By default, when you install a package, it goes to your system-wide Python installation. This means that over time you will install a bunch of packages with conflicting versions for different projects. At that point, it's very likely that certain packages will stop working, and your best bet is to to reinstall everything and start over from scratch. This problem is so common (not only in Python) that it's been given a name: "[dependency hell](https://en.wikipedia.org/wiki/Dependency_hell)".

If you use `virtualenv`s instead of the system-wide install, you can create a new environment for each project, install your packages there, and avoid this problem altogether! If you run into any packaging problems it's easy to throw a virtualenv away and recreate it. It's slightly more work, but will save you time and frustration later! Virtualenvs are also commonly used when deploying Python applications in a production environment.

The basic command you need to get started is the one that creates a new virtualenv: `virtualenv path_to_directory`. I usually create my virtualenvs in my home directory with the name `projectname_env`. This is just my personal preference, feel free to place them or name them with whatever makes sense to you.

```
$ virtualenv ~/someproject_env
New python executable in /home/Steven/someproject_env/bin/python
Installing setuptools, pip...done.
```

After that, to use the virtualenv you just refer to the `python` bin inside the virtualenv instead of the system-wide `python` command we have been using up until now. Compare the contents of `sys.path` (the list of directories used by the interpreter to locate packages) inside and outside the virtualenv:

```
$ ~/someproject_env/bin/python
>>> import sys
>>> sys.path
['', '/home/Steven/someproject_env/lib/python2.7', ...]

$ python
>>> import sys
>>> sys.path
['', '/usr/lib/python2.7', ...]
```

The virtualenv will automatically pick up packages under the virtualenv and ignore packages installed in the system-wide Python install. This is what we want.

To remove a virtualenv, just delete the directory. You shouldn't be storing any of your own work in the virtualenv, so it should be safe to remove or recreate at any time.

That's basically all you need to start using virtualenv! If you are allergic to typing `~/foo_env/bin/python` before every command, there are some helpers you can use: the [activate](http://virtualenv.readthedocs.org/en/latest/userguide.html#activate-script) script, or the [virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/) and [autoenv](https://github.com/kennethreitz/autoenv) projects. I prefer the full path because it's not that much typing, it always does the correct thing, and it doesn't rely on additional tooling or implicit state to work.

Now that we have a virtualenv, let's install some packages into it!

## Pip: Installing and managing packages

Inside our virtualenv, we should have a `pip` command:

```
$ something_env/bin/pip --version
pip 6.1.1 from /home/Steven/something_env/lib/python2.7/site-packages (python 2.7)
```

Pip uses a number of subcommands, let's walk through the major ones. First, we can use `install` to install a new package by name:

```
$ something_env/bin/pip install flask
Collecting flask
  Downloading Flask-0.10.1.tar.gz (544kB)
    100% |################################| 544kB 512kB/s
[...]
Successfully installed Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.4 flask-0.10.1 itsdangerous-0.24
```

Next, we can use `uninstall` to remove a package:

```
$ something_env/bin/pip uninstall flask
Uninstalling Flask-0.10.1:
  /home/Steven/something_env/lib/python2.7/site-packages/Flask-0.10.1-py2.7.egg-info
  /home/Steven/something_env/lib/python2.7/site-packages/flask/__init__.py
  [...long listing of all the files in the package...]
Proceed (y/n)? y
  Successfully uninstalled Flask-0.10.1
```

Next, we can use `freeze` to print out a list of all packages and what version they're at:

```
$ something_env/bin/pip freeze
Flask==0.10.1
itsdangerous==0.24
Jinja2==2.8
MarkupSafe==0.23
Werkzeug==0.11.4
```

This version information is useful when you want to share your project with another person (e.g. making it open source, or sharing with a coworker) or when you want to deploy the project to another machine. These versions are the specific dependencies needed to run your project. You can dump this information to a simple [requirements text file](https://pip.pypa.io/en/stable/user_guide/#requirements-files) or specify them in [setup.py](http://python-packaging-user-guide.readthedocs.org/en/latest/requirements/#install-requires).

Lastly, use `pip help` to get more information on other subcommands and their usage.

Now, how do we find what packages are available?

## PyPI: The Python Package Index

Notice above that we just specified "flask" and the `pip install` command automatically knew what package we were talking about. How did `pip` know that? By default, `pip` knows how to search [PyPI](https://pypi.python.org/pypi), the Python Package Index. PyPI is the official central repository of packages run by the Python Software Foundation. It is where you go to find most open sources packages.

Let's look at [Flask's entry on PyPI](https://pypi.python.org/pypi/Flask). We can see a brief description of the package, some links to the homepage / source code / documentation, information on the latest version, a link to download the package, and some additional metadata like what platform it supports or what categories it falls under. The `pip` command uses PyPI's API to search this package information, download the files, and install them for you.

If you aren't sure of a package's name, try searching for it on PyPI to confirm the name, then `pip install` it. If you're trying to find a new package to solve some problem you're having, you could try searching PyPI for a few keywords, browsing the [categories](https://pypi.python.org/pypi?%3Aaction=browse), looking at popularity rankings on [pypi-ranking.info](http://pypi-ranking.info/alltime) or [py3readiness.org](http://py3readiness.org/), or searching Google / StackOverflow / mailing lists / etc. for package recommendations.

## Installing packages from source

Not all packages are on PyPI. Sometimes a package may only exist on a site like GitHub, Bitbucket, or some other random website, or in a source code repository, or in an archive on your local machine. In these cases, you need to install the package directly "from source", outside of PyPI.

Another case when you may want to install a package from source is when you want to try an in-development or unreleased version of a package that hasn't made it to PyPI yet.

The first thing to look for is if the thing you're trying to install contains a `setup.py` file. If it does, you should be able to use `pip install` on the containing directory:

```
$ cd mypkg
$ ls -al setup.py
-rw-r--r-- 1 Steven Steven 121 Mar  6 20:18 setup.py

$ cat setup.py
from setuptools import setup, find_packages
setup(
    name="mypkg",
    version="0.1",
    packages=find_packages(),
)

$ ~/something_env/bin/pip install .
Processing /home/Steven/something_env/mypkg
Installing collected packages: mypkg
  Running setup.py install for mypkg
Successfully installed mypkg-0.1
```

You can also run `pip install` on an archive (`.zip`, `.tgz`, `.tar.gz`):

```
$ ~/something_env/bin/pip install pkg.tar.gz
Processing ./pkg.tar.gz
Installing collected packages: mypkg
  Running setup.py install for mypkg
Successfully installed mypkg-0.1
```

If the thing you're trying to use doesn't have a `setup.py`, then it may not be a real Python package, it could just be a collection of source files or a standalone script. In that case, try putting the code in a subdirectory next to your code and `import` it like a module.
