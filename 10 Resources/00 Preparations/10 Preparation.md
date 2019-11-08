# Preparations

Welcome to Data Processing. We will be using a variety of tools, which will
require some initial configuration. While some of this will likely be dull,
doing it now will enable us to do more exciting work in the weeks that follow
without getting bogged down in further software configuration.

## Getting a text editor

Those of you who have taken CS50 are used to working from within the IDE. In
this course, we remove those training wheels. This means you have to download
and install a text editor on your own computer. Free code editors are, for
example, [Atom] or [Sublime Text] (limitless trial for Windows/Mac/Linux).

[Atom]: https://atom.io/
[Sublime Text]: https://sublimetext.com/3

## Installing Python

In this assignment we will be working with Python 3 to gather and process
datasets.

- If you are a Windows user and have no pre-existing Python installation go to
  [Python](http://www.python.org) and install Python 3.7.x.

- For users of OS X, you already have Python so you need to check which
  version. You do this by opening a Terminal (included in every Mac) and typing
  `python` followed by pressing return. This will start Python. If you don't
  have Python 3 or up, you should install it (i.e. download it from the
  [Python](http://www.python.org) and install it).

- If you are using a version of Linux, a recent version of Python is likely
  already installed. If it's not, you can install it through the package
  manager of your Linux distribution.

Once you successfully installed Python, you will be able to run `python` from
your terminal. On Windows you can get a command line by starting the "Command
Prompt", and on a Mac by starting the "Terminal".


## Installing the BeautifulSoup Library

The BeautifulSoup library is a library that supports web­scraping and
-crawling. To install the library, either go to their website and follow the
instructions, or install it via the command line using `pip install
beautifulsoup4`. Depending on which Python version(s) is/are installed on your
machine, it might be that `pip` installs it for Python version 2.x. If that is
the case, `pip3` is a likely candidate to help you install BeautifulSoup for
version 3.x. On the website you can download the library in a zip file. Simply
extract this zip, open a terminal in the directory where the unpacked files are
and execute the command ( *from the command line, not from inside Python or
IDLE* )

	~$ python setup.py install

This will run the script stored in "setup.py" which installs the library.

Make sure that you run this with administrator privileges (add `sudo` at the
front on Mac and Linux and use and Administrator command prompt on Windows)!

If you are using windows and get the error that pip is not a known command, try using `python -m pip install beautifulsoup4` instead. 


## Executing Python Code

Being an interpreted language, Python requires an interpreter to execute a
script. You’ve already executed a script for the BeautifulSoup installation!
The interpreter is called by typing python at the command line. If a file is
passed as the first argument (e.g., `python myfile.py`), the interpreter will
execute the specified script. If no parameters are given, the interpreter will
launch in interactive mode where you can type individual commands one at a
time. Interactive mode is similar to using the prompt in Matlab in the sense
that variables can be created, functions can be called, etc. In general, most
things that can be done via script can be done in interactive mode. However, in
practice, scripts are more common.

Therefore the process is you edit a file, e.g., `hello_world.py` in the editor
of your choice (Atom, Sublime Text 3, Emacs, Notepad++, or a complete IDE like
PyDev) You run python and give your script as a parameter. Here is a simple
example.

	import math

	letter = math.ceil(math.pow(3, 2) * 11 - 2)
	print("Hello World, and welcome to D" + chr(letter) + "t" + chr(letter) + "!")

If this code example does not run on your machine, a common cause (if you use
Windows) is that the environment variables of your operating system are not
correctly set. If you follow along with these
[instructions](https://superuser.com/questions/143119/how-do-i-add-python-to-the-windows-path) you can change your environment variables.

To get your feet wet with python go through [this](https://docs.python.org/3/tutorial/introduction.html) and [this](https://docs.python.org/3/tutorial/controlflow.html) page of the python
tutorial and play around with strings, lists, control flow, functions, etc.


## Try if BeautifulSoup is correctly installed

Copy the following code to a script and run it with python:

	from bs4 import BeautifulSoup

	html_doc = """
    <html><head><title>The Dormouse's story</title></head>
    <body>
    <p class="title"><b>The Dormouse's story</b></p>

    <p class="story">Once upon a time there were three little sisters; and their names were
    <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>

    <p class="story">...</p>
    """
	soup = BeautifulSoup(html_doc, 'html.parser')

    print(soup.title)

If you get output similar to this, everything is fine:

	$ python bs4_test.py
	# <title>The Dormouse's story</title>

If this is not the case, there are several fixes described
on the documentation website. If you’re still
having trouble getting things working just ask for help.

A common error message is that a package called 'Requests' is not installed. If this is the case, install it using `pip` as you have installed BeautifulSoup:
`pip install requests` from the terminal or `python -m pip install requests` from the command prompt.

## Installing Matplotlib

The last library needed for this week is Matplotlib. You can do so by using pip (Pip Installs Packages), by entering the following line of code on the command line of your terminal:

`pip install matplotlib`

If you are using both Python 2 and Python 3 on your machine, there is a good chance that the keyword `pip` is reserved for Python 2 installations. If that is the case, replace `pip` with `pip3`.

pip will most likely also install a lot of 'dependencies' (other libraries Matplotlib requires in order to function), so not to worry if this happens!

In order to test whether or not Matplotlib is correctly installed, try running the following piece of code:

	import matplotlib.pyplot as plt

	plt.plot([1,2,3,4])
	plt.ylabel('some numbers')
	plt.show()

If you see an image similar to the one below, Matplotlib is correctly installed.

![Matplotlib Example](mplexample.png)
