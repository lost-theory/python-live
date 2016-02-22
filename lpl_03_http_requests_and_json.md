## HTTP requests and JSON

One very common task you will face while programming is getting two systems to talk to each other. Either you want to pull some data from an existing service into a program, save it to disk, send data from one system to another, etc.

Since many systems are on the web, it's very common to do this communication via **HTTP requests**. You can use Python to make the same requests you would make in a web browser. Many of these requests use **JSON**, which is a common format for exchanging data between two systems. Let's look at making HTTP requests first, then look at how to use JSON with web APIs.

## Making HTTP requests

Let's visit the home page of Wikipedia in our browser:

https://en.wikipedia.org/wiki/Main_Page

Right click on the page and click "View source" or "View page source". This is the underlying HTML, CSS, and JavaScript code for the page you are viewing. We can make the same request in Python by using the `urlopen` function (provided by [`urllib`](https://docs.python.org/2/library/urllib.html) in Python 2 and [`urllib.request`](https://docs.python.org/3/howto/urllib2.html) in Python 3):

```python
>>> import urllib
>>> response = urllib.urlopen("https://en.wikipedia.org/wiki/Main_Page")
>>> response
<addinfourl at 7696578914712 whose fp = <socket._fileobject object at 0x6ffffe02ed0>>
>>> dir(response)
['__doc__', '__init__', '__iter__', '__module__', '__repr__', 'close', 'code', 'fileno', 'fp', 'getcode', 'geturl', 'headers', 'info', 'next', 'read', 'readline', 'readlines', 'url']
```

> **Note**: The use of `urlopen` here is just to get us up and running quickly using Python's standard library. In the "Installing open source libraries" chapter we will learn how to install the `requests` package, which is much more powerful than `urlopen` and just as simple to use, particularly for sending data via HTTP.

The `response` object returned by `urlopen` is what is called a "file-like" object. From the `dir` call we can see it has many of the same methods as the `file` type, like `read` and `close`. Let's try `read`-ing it:

```python
>>> page = response.read()
>>> len(page)
66739
>>> page
'<!DOCTYPE html>\n<html lang="en" dir="ltr" class="client-nojs">\n<head>\n<meta charset="UTF-8" />\n<title>Wikipedia, the free encyclopedia</title>\n<script>document.documentElement.className........[snipped 66k of text].........</html>\n'
```

If you run that code, you'll see the string is quite large! That's 66k characters = ~66 kilobytes of text. Let's try to whittle it down to something more useful. If we view the page in our browser, we see a box called "On this day..." which lists interesting events that happened on this day in history. Let's try to write some Python code to grab this information. If we view the source around that box, we see it starts here:

```html
<h2 id="mp-otd-h2" style="[snip]"><span class="mw-headline" id="On_this_day...">On this day...</span></h2>
```

Let's see if we can find this section in the `page` string. First, let's search for the text "On_this_day", and then slice off the next 500 characters, just to see what it looks like:

```python
>>> print page[start:start+500]
On_this_day...">On this day...</span></h2>
</td>
</tr>
<tr>
<td style="color:#000; padding:2px 5px 5px;">
<div id="mp-otd">
<p><b><a href="/wiki/February_21" title="February 21">February 21</a></b>: <b><a href="/wiki/International_Mother_Language_Day" title="International Mother Language Day">International Mother Language Day</a></b>; <b><a href="/wiki/Language_Movement_Day" title="Language Movement Day">Language Movement Day</a></b> in Bangladesh</p>
<div style="float:right;margin-left:0.5em;"
```

We're starting to see some of the text we're interested in. How do we find the end point? If we go back to "View source" in our browser, it looks like the list of bullet points stops at the text "More anniversaries:", but let's grab it a slightly different way. Let's try the following:

* First, run `split` on "On_this_day" and grab the **right** half of the split text. This is the start of the text we're interested in all the way to the end of the page.
* Second, run `split` on "More anniversaries:" and grab the **left** half on the split text. This is just the text we're interested in, the contents of the "On this day" box!

```python
>>> box_to_end_of_page = page.split("On_this_day")[1]
>>> box = box_to_end_of_page.split("More anniversaries:")[0]
>>> len(box)
3520
>>> print box
...">On this day...</span></h2>
</td>
</tr>
<tr>
<td style="color:#000; padding:2px 5px 5px;">
<div id="mp-otd">
<p><b><a href="/wiki/February_21" title="February 21">February 21</a>...
<div style="float:right;margin-left:0.5em;" id="mp-otd-img">
<div class="thumbinner mp-thumb" style="background: transparent; bord...
<div class="thumbcaption" style="padding: 0.25em 0; word-wrap: break-...
</div>
</div>
<ul>
<li><a href="/wiki/1543" title="1543">1543</a> – Led by the <a href="...
<li><a href="/wiki/1804" title="1804">1804</a> – Built by <a href="/w...
<li><a href="/wiki/1919" title="1919">1919</a> – <a href="/wiki/Bavar...
<li><a href="/wiki/1958" title="1958">1958</a> – British artist <a hr...
<li><a href="/wiki/1973" title="1973">1973</a> – After accidentally h...
</ul>
<ul style="list-style:none; margin-left:0;">
<li>
```

This is called **screen scraping**. This approach can give us the data we're interested in, but it's a little hard to use due to the (sometimes incomplete) HTML tags, and the way we're finding the text is very brittle. If any content of the page changes, it can easily break our code. For example, what if they renamed "On_this_day" to "Today_in_history", or if they removed the "More anniversaries:" text? Our program would stop working and throw an error.

It would be better if there was an official method to retrieve data from Wikipedia programmatically and in a way that gives us just the data we need, in a format that is easy for us to process. Thankfully, Wikipedia and many other websites and services provide such a thing!

## APIs

In addition to serving up the requests made by web browsers, some sites and services provide an **API**: **application programming interface**. APIs are typically a documented list of "endpoints" or "actions" with an explanation on how to pass input to / collect output from those endpoints. Unlike the Wikipedia homepage, these API actions are meant to be called and consumed programmatically. Think of them as "function calls" into an external system. For example, Wikipedia provides a web API for much of its data and functionality:

https://www.mediawiki.org/wiki/API:Main_page

That page gives you an overview of what the API does, what format of data it uses for input and output, and how to start making calls. On the right side, it gives a list of all the actions the API can perform, such as:

* Authentication
* Queries
* Searching
* Creating and editing pages
* Uploading files
* etc.

The major pieces of functionality exposed in Wikipedia's user interface have a corresponding API call that can be controlled programmatically.

> **Note**: An API doesn't need to be connected to a web service. There are many non-web APIs. Many libraries, including the ones in Python's stdlib, are essentially APIs. For example, the `open` function and the [os.path](https://docs.python.org/2/library/os.path.html#module-os.path), [shutil](https://docs.python.org/2/library/shutil.html#module-shutil), etc. modules can be considered Python's "filesystem API".

## Let's make an API call

Let's take a look at an example API call, the [categorymembers](https://www.mediawiki.org/wiki/API:Categorymembers) call. In the call's documentation we see a description of what the call does, a list of parameters it takes, some example calls, and some information on what errors it can return. It says that it returns a list of pages in a given category.

This is all we need to start playing around with the call in Python. Before we do that though, let's try the first example URL in our browser:

https://en.wikipedia.org/w/api.php?action=query&list=categorymembers&cmtitle=Category:Physics

It's returning an HTML page which has a listing of data for debugging purposes, and that if we want the "non-HTML representation of the JSON format, set format=json". Let's try that by adding `&format=json` to the end of the URL:

https://en.wikipedia.org/w/api.php?action=query&list=categorymembers&cmtitle=Category:Physics&format=json

Let's put that in a `urlopen` call:

```python
>>> res = urllib.urlopen("https://en.wikipedia.org/w/api.php?action=query&list=categorymembers&cmtitle=Category:Physics&format=json")
>>> res.read()
'{"batchcomplete":"","continue":{"cmcontinue":"page|46454c444d414e2c2048554d450a48554d452046454c444d414e|48407923","continue":"-||"},"query":{"categorymembers":[{"pageid":22939,"ns":0,"title":"Physics"},{"pageid":22688097,"ns":0,"title":"Branches of physics"},{"pageid":3445246,"ns":0,"title":"Glossary of classical physics"},{"pageid":24489,"ns":0,"title":"Outline of physics"},{"pageid":1653925,"ns":100,"title":"Portal:Physics"},{"pageid":33327002,"ns":0,"title":"Cabbeling"},{"pageid":151066,"ns":0,"title":"Classical physics"},{"pageid":19725090,"ns":0,"title":"Cold"},{"pageid":47723069,"ns":0,"title":"Epicatalysis"},{"pageid":685311,"ns":0,"title":"Experimental physics"}]}}'
```

Now what do we do? It kind of looks like a Python dictionary with some data nested further inside, but it's all mashed into a string. How do we get the data out of the string? As the documentation mentioned, this is the "JSON" formatted data, what is JSON?

## Using JSON

**JSON** stands for "**JavaScript Object Notation**". It is JavaScript's syntax for writing strings, numbers, lists, etc. Since JavaScript runs natively in the web browser, it is one of the most popular data formats. Every language (Python, PHP, C++, Java, etc.) has a way to convert to and from JSON.

It also looks a lot like Python's syntax, but there are some slight differences. Because of those differences, we need to use Python's `json` module to decode and encode to/from Python's data types. There are basically two methods you need to know:

* `json.dumps`: encodes a Python object into a JSON string
* `json.loads`: decodes a JSON string into Python (`dict`, `list`, `int`, `float`, `str`)

Examples:

```python
>>> python_dict = {"pets": [{"name": "Whiskers", "type": "cat", "owners": ["Alice", "Bob"], "age": 3}]}
>>> #let's convert it to JSON
>>> json_str = json.dumps(python_dict)
>>> json_str
'{"pets": [{"age": 3, "owners": ["Alice", "Bob"], "type": "cat", "name": "Whiskers"}]}'
>>> #now it's a JSON encoded string.. let's try to convert it back to python
>>> json.loads(json_str)
{u'pets': [{u'age': 3, u'type': u'cat', u'owners': [u'Alice', u'Bob'], u'name': u'Whiskers'}]}
>>> json.loads(json_str) == python_dict
True
>>> #round-trip encoding and decoding should produce the same object, if all the data types are compatible
```

JSON is not only used to talk to the web, it is commonly used in pure Python or purely serverside code, e.g. to read and write Python data structures to a file or database. See the [`load`](https://docs.python.org/2/library/json.html#json.load) and [`dump`](https://docs.python.org/2/library/json.html#json.dump) functions, which do the same thing as `loads` and `dumps`, but with file objects instead of strings.

## Talking to a JSON web API

Let's try `json.loads` on the data we previously grabbed from the API call.

```python
>>> import urllib
>>> import json
>>> res = urllib.urlopen("https://en.wikipedia.org/w/api.php?action=query&list=categorymembers&cmtitle=Category:Physics&format=json")
>>> pages = json.loads(res.read())
>>> pages
{u'batchcomplete': u'', u'query': {u'categorymembers': [{u'ns': 0, u'pageid': 22939, u'title': u'Physics'}, {u'ns': 0, u'pageid': 22688097, u'title': u'Branches of physics'}, {u'ns': 0, u'pageid': 3445246, u'title': u'Glossary of classical physics'}, {u'ns': 0, u'pageid': 24489, u'title': u'Outline of physics'}, {u'ns': 100, u'pageid': 1653925, u'title': u'Portal:Physics'}, {u'ns': 0, u'pageid': 33327002, u'title': u'Cabbeling'}, {u'ns': 0, u'pageid': 151066, u'title': u'Classical physics'}, {u'ns': 0, u'pageid': 19725090, u'title': u'Cold'}, {u'ns': 0, u'pageid': 47723069, u'title': u'Epicatalysis'}, {u'ns': 0, u'pageid': 685311, u'title': u'Experimental physics'}]}, u'continue': {u'continue': u'-||', u'cmcontinue': u'page|46454c444d414e2c2048554d450a48554d452046454c444d414e|48407923'}}
```

Now we have a Python `dict` called `pages` that has the data from the API response! We can now use this to start doing useful things with the data. The format is fairly dense, because it's capturing quite a bit of data and metadata for us to use. To figure out what to do with this data, let's poke at it a bit:

```python
>>> #what type is it?
>>> type(pages)
<type 'dict'>
>>> #it's a dict, how many keys does it have?
>>> len(pages)
3
>>> #3 keys.. not too many, what are the keys?
>>> pages.keys()
[u'batchcomplete', u'query', u'continue']
>>> #let's look at each one:
>>> pages['batchcomplete']
u''
>>>
>>> #not very interesting, next?
>>> pages['query']
{u'categorymembers': [{u'ns': 0, u'pageid': 22939, u'title': u'Physics'}, {u'ns': 0, u'pageid': 22688097, u'title': u'Branches of physics'}, {u'ns': 0, u'pageid': 3445246, u'title': u'Glossary of classical physics'}, {u'ns': 0, u'pageid': 24489, u'title': u'Outline of physics'}, {u'ns': 100, u'pageid': 1653925, u'title': u'Portal:Physics'}, {u'ns': 0, u'pageid': 33327002, u'title': u'Cabbeling'}, {u'ns': 0, u'pageid': 151066, u'title': u'Classical physics'}, {u'ns': 0, u'pageid': 19725090, u'title': u'Cold'}, {u'ns': 0, u'pageid': 47723069, u'title': u'Epicatalysis'}, {u'ns': 0, u'pageid': 685311, u'title': u'Experimental physics'}]}
>>> #ok, that looks interesting! it's another dict, what are its keys?
>>> pages['query'].keys()
[u'categorymembers']
>>> #just one key, so let's go down another level
>>> pages['query']['categorymembers']
[{u'ns': 0, u'pageid': 22939, u'title': u'Physics'}, {u'ns': 0, u'pageid': 22688097, u'title': u'Branches of physics'}, {u'ns': 0, u'pageid': 3445246, u'title': u'Glossary of classical physics'}, {u'ns': 0, u'pageid': 24489, u'title': u'Outline of physics'}, {u'ns': 100, u'pageid': 1653925, u'title': u'Portal:Physics'}, {u'ns': 0, u'pageid': 33327002, u'title': u'Cabbeling'}, {u'ns': 0, u'pageid': 151066, u'title': u'Classical physics'}, {u'ns': 0, u'pageid': 19725090, u'title': u'Cold'}, {u'ns': 0, u'pageid': 47723069, u'title': u'Epicatalysis'}, {u'ns': 0, u'pageid': 685311, u'title': u'Experimental physics'}]
>>> #now we see a list, what's the first element of the list?
>>> pages['query']['categorymembers'][0]
{u'ns': 0, u'pageid': 22939, u'title': u'Physics'}
>>> #title looks like the most interesting thing here, let's grab it
>>> pages['query']['categorymembers'][0]['title']
u'Physics'
>>> #from this little experiment we've figured out how to grab a title from each page
>>> #let's try grabbing all the titles
>>> for page in pages['query']['categorymembers']:
...     print page['title']
...
Physics
Branches of physics
Glossary of classical physics
Outline of physics
Portal:Physics
Cabbeling
Classical physics
Cold
Epicatalysis
Experimental physics
```

This is a pretty common way to explore and use the result from an API call. Here's one other shortcut you can try: you'll notice that in the original HTML version of this API call it printed out [a nicely formatted listing](https://en.wikipedia.org/w/api.php?action=query&list=categorymembers&cmtitle=Category:Physics) with line breaks and indentation for the different levels. Python can do the same thing using the `pprint` module (short for "pretty print"), and we can apply it at any level of the data to quickly drill down to the important information:

```python
>>> import pprint
>>> pprint.pprint(pages['query'])
{u'categorymembers': [{u'ns': 0, u'pageid': 22939, u'title': u'Physics'},
                      {u'ns': 0,
                       u'pageid': 22688097,
                       u'title': u'Branches of physics'},
                      {u'ns': 0,
                       u'pageid': 3445246,
                       u'title': u'Glossary of classical physics'},
                      {u'ns': 0,
                       u'pageid': 24489,
                       u'title': u'Outline of physics'},
                      {u'ns': 100,
                       u'pageid': 1653925,
                       u'title': u'Portal:Physics'},
                      {u'ns': 0, u'pageid': 33327002, u'title': u'Cabbeling'},
                      {u'ns': 0,
                       u'pageid': 151066,
                       u'title': u'Classical physics'},
                      {u'ns': 0, u'pageid': 19725090, u'title': u'Cold'},
                      {u'ns': 0,
                       u'pageid': 47723069,
                       u'title': u'Epicatalysis'},
                      {u'ns': 0,
                       u'pageid': 685311,
                       u'title': u'Experimental physics'}]}
>>> pprint.pprint(pages['query']['categorymembers'])
[{u'ns': 0, u'pageid': 22939, u'title': u'Physics'},
 {u'ns': 0, u'pageid': 22688097, u'title': u'Branches of physics'},
 {u'ns': 0, u'pageid': 3445246, u'title': u'Glossary of classical physics'},
 {u'ns': 0, u'pageid': 24489, u'title': u'Outline of physics'},
 {u'ns': 100, u'pageid': 1653925, u'title': u'Portal:Physics'},
 {u'ns': 0, u'pageid': 33327002, u'title': u'Cabbeling'},
 {u'ns': 0, u'pageid': 151066, u'title': u'Classical physics'},
 {u'ns': 0, u'pageid': 19725090, u'title': u'Cold'},
 {u'ns': 0, u'pageid': 47723069, u'title': u'Epicatalysis'},
 {u'ns': 0, u'pageid': 685311, u'title': u'Experimental physics'}]
```

Any time you see a big blob of confusing data, try throwing `pprint.pprint` at it! From there, start looking for anything of interest using indexing or slicing, put that new result into `pprint.pprint`, and repeat. If you get comfortable with this even the largest and nestiest of data structures will fall before you!

## Building on top of an API

So now we know how to pull the first 10 titles from the "Physics" category in Wikipedia as JSON and covert that into a Python object. That's pretty neat, but what next? What about other categories? What if I want more than 10 titles? What can I do with those titles?

To do those things we need to build on top of this simple API call, for example by doing the following:

1. We can make a function that takes a category name, uses the `format` method to substitute the category into the URL, makes the call, parses the JSON, and return the result. This way we don't need to hardcode the "Physics" category directly into the URL or parse the JSON each time we want to make the call.

2. In the [API documentation](https://www.mediawiki.org/wiki/API:Categorymembers) for this call, it explains the `cmlimit` and `cmcontinue` parameters, which control how many results the call returns and how to continue to the next page of results. This is sometimes called "pagination". On the documentation for the "[query](https://www.mediawiki.org/wiki/API:Query)" API calls (scroll down to "Continuing queries") it explains that many of the API calls, including `categorymembers`, use this technique for fetching more results, so we could write another function to handle this.

3. The page titles can be passed into other API calls (e.g. [grabbing the page contents as text](https://en.wikipedia.org/w/api.php?action=query&prop=revisions&rvprop=content&format=jsonfm&titles=Physics)), or we can use it to build the [link to the page on Wikipedia](https://en.wikipedia.org/wiki/Physics) for reading. Again, we can write functions to do these things.

As we write these functions, we would be making our own API on top of the Wikipedia API. This is called "[abstraction](https://en.wikipedia.org/wiki/Abstraction_(computer_science))" and it is very common in programming. When you program in Python, you are writing APIs and abstractions on top of other abstractions and so on until you get down to the hardware level.

If you're interested in learning more about abstraction, writing your own APIs, or how to communicate with a web service, I highly recommend you try implementing some of these functions.

If you're interested in jumping right into using the Wikipedia data, someone [has already built a Python library to do that](https://pypi.python.org/pypi/wikipedia). We will learn how to install these libraries in the "Installing open source libraries" chapter.

In general, if you want to interact with some kind of web service, website, system, or API, try the following:

* Google for "\<name\> python" or "\<name\> python library" (e.g. "[wikipedia python library](https://www.google.com/search?q=wikipedia+python+library)"). This will show you if someone has already written a Python library, script, blog post, etc. that does what you want to do.
* Google for "\<name\> API". This will show you if there is a documented API or library in another language that you could write your own library against.
* If either of those fail, you may have to resort to screen scraping, reverse engineering, contacting the owner of the service, or asking around in the community. Also, if you end up scraping or reverse engineering a website, be sure to read the site's [robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard) and terms of use.
* You could also repeat the same process for competitors, similar sites, or try to gain access to the system or its data in some other way. For example, say Wikipedia didn't provide an API and scraping wasn't possible. Could you try another online encyclopedia? Could you get a ["dump" of the data](https://dumps.wikimedia.org/) from Wikipedia and build an API that way?
* Another way to find APIs is to try an API directory site like [ProgrammableWeb](http://www.programmableweb.com/apis/directory).
