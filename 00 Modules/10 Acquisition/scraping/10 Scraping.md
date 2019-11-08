# Scraping

Web scraping is a way of extracting data from websites. While this process could be done manually (by reading information on a website, and then writing that information down in a file) it is usually done through the use of software. Scraping can be a valuable tool for extracting data, when a website does not give you an option to download the content, either through an API or a direct download link. One example of such a website is the IMDB page that we will be scraping in this exercise.

In this assignment you will learn to use the Document Object Model (DOM) using Python via the BeautifulSoup library. We provide some scaffolding for the programming exercise in this week's homework. We will be trying to answer the question: Between 2008 and 2017, were there any years in which movies (from the top 50) scored significantly higher than other years within this timeperiod?

* The IMDB highest rated movies exercise: [moviescraper.py]

[moviescraper.py]: moviescraper.py

## DOM scraping and traversal

To scrape data, we will be using BeautifulSoup, a Python web mining module. Its
description is as follows: _Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic > ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work._

Instructions:

1. We assume Python 3, Requests and BeautifulSoup4 are installed (see [preparations] if this is not
the case).

2. We will be looking at IMDB movies and getting data off this website. To
get started, you should look at [the BeautifulSoup documentation].

3. To get you started we have provided you with a script [moviescraper.py] that
loads the correct IMDB address, makes a local backup of it (`movies.html`)
and outputs a CSV file (`movies.csv`) that will contain only a header until
you complete the implementation of the functions `extract_movies(dom)` and
`save_csv(outfile, movies)`.
If you want to check your `movies.csv` file in Excel on a Mac, add 'sep=,' as the first line of the file. This way the file is parsed correctly in columns.

4. To help you validate your script we provide an example output CSV
file [output.csv]. You should run the `moviescraper.py` script with the command
`python moviescraper.py`. This will copy the IMDB webpage to the current directory
and save a CSV file. The example output file only contains the first two movies.

5. When you hand this exercise in be sure to submit: your `moviescraper.py`,
`movies.html` and `movies.csv`. This will allow us to verify that your
output CSV file is correct and that the script actually works given the HTML
from IMDB.

6. It could be that there are missing data (for instance the runtime). Insert
   an appropriate value when something is missing. Note that the director is not an actor or actress.

[output.csv]: output.csv
[preparations]: /resources/preparations
[the BeautifulSoup documentation]: https://www.crummy.com/software/BeautifulSoup/bs4/doc/

### Building `moviescraper.py`

This is the introductory exercise to BeautifulSoup. We will try to guide you along as
much as possible, but you should read up on documentation and get used to doing
that. It's a really useful skill and a big part of programming is
self-­learning!

This is also just a scaffolding, so you actually don't have to use this at all. As
long as your code runs at the end of the day and produces the right results in
a CSV file, we're happy.

`print()` is probably going to be your best friend for debugging, so print often
especially if something goes wrong.

Take a look at the following attributes, taken from the documentation, that show some basic functionalities of BeautifulSoup.

        soup.title
        # <title>The Dormouse's story</title>

        soup.title.name
        # u'title'

        soup.title.string
        # u'The Dormouse's story'

        soup.title.parent.name
        # u'head'

        soup.p
        # <p class="title"><b>The Dormouse's story</b></p>

        soup.p['class']
        # u'title'

        soup.a
        # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

        soup.find_all('a')
        # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
        #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
        #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

        soup.find(id="link3")
        # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>


## Visualizing the data

Now that we have the data in a `.csv`-format, it is time to try to get some insights into what we scraped. For this you will be using [visualizer.py] as scaffolding.

[visualizer.py]: visualizer.py

For now, the main function only prints the variable `data_dict` and it's type, to give you some insight into what the variable is.

Your aim in this part of the exercise is to visualize the data scraped from IMDB in a line chart. In order for the data to be plotted in a line chart, the data has to be transformed. Find the average rating a movie in the top 50 of IMDB has gotten for the years 2008-2017. Potentially interesting things to look at for this part of the exercise are the [Python documentation on dictionaries] and [csv], especially the `DictReader` class could come in handy. If you feel that there is perhaps a better way to answer our question using your data, feel free to **also** create any other plots.

[Python documentation on dictionaries]: http://docs.python.org/3/tutorial/datastructures.html#dictionaries
[csv]: https://docs.python.org/3/library/csv.html

Once the data is in the correct format, you can plot it using Matplotlib. You can read [this tutorial] on some of the basics of plotting data using Matplotlib. Of course, there is much more documentation to be found online, which can either help you on your way with plotting the data, or make your already existing plot more aesthetically pleasing or more effective.

[this tutorial]: https://matplotlib.org/users/pyplot_tutorial.html

Reflect on the figure your script creates. How do you choose the range of the y-axis? Are all elements present in the figure? Think about the consequences of the design choices you make.

Don't forget to keep an eye on code design. Use functions, choose useful names for your variables, prevent code repetition, place comments, etc.


## Bonus exercises

If you have finished the scraper and visualization before the deadline and are itching to gather more data, you can take a look at the bonus exercise: [Crawler]

If the Python data analysis and visualization is more your cup of tea, you can have a look at the [Pandas] library to see what kind of things you can do with the data gathered using the scraper, that are beyond the scope of this exercise.

[Crawler]: /homework/crawling
[Pandas]: https://pandas.pydata.org/


## Submitting

In this course you will use GitHub to submit your code and all other documents. You will also use GitHub Pages to publish your visualizations.


### Git

Please attend the lecture on Introduction to GitHub which will guide your through the setup and basic git commands. When you are all set-up follow the instructions below.


### Github

Structure your assignments folder as follows: make two subfolders *Homework* and *Design*. In each folder you will weekly make a new folder with a week number and put your documents from that week in there. Specific instructions on what should be committed each week are to be found in the assignments description. For more information on the lay-out of your repository, see the documentation in the menu on the left under 'Resources'.

__Submit the URL of your repository__ below. Make sure your submit a link to the root of your repository, i.e. https://github.com/*username*/dataprocessing

Please may refer to this [Github manual] if you forgot your commands.

[Github manual]: Github_manual.pdf



Add a folder Week_1 to your DataProcessing/Homework repository containing the following files:

1. `moviescraper.py`
2. `visualizer.py`
3. `movies.html`
4. `movies.csv`
