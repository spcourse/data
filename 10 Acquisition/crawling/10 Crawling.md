# Crawling

## Web crawling & HTML scraping

This exercise, we will practice scraping from multiple websites using web-crawling. You will learn how you can acquire data from multiple pages by following links. You’ll be scraping data from IMDB, the Internet Movie DataBase!

For this exercise, we have provided you with some code that makes HTML backups of a few selected webpages. You will write code that scrapes IMDB's top 250 movies index page and the individual movie pages yourself.

Download a zip file with all necessary files [here](crawler_files.zip). This .zip file contains two files:

* With `imdb_crawler.py` you'll scrape multiple IMDB pages to create a CSV file with information from top 250 IMDB movies
* You can use `sample_HTML.csv` as an example. The formatting of your output file should be the same as in this file. 

You should submit your version of `imdb_crawler.py` script along with the output CSV file (`top250movies.csv`), and the two backup HTML files that the script creates (`index.html` and `movie-083.html`).

The goal of this exercise is to create a CSV file containing the following data for each movie in the top 250:

* Title of movie
* Runtime
* Genre (separated by semicolons if multiple)
* Director(s) (separated by semicolons if multiple)
* Writer(s) (separated by semicolons if multiple)
* Actors (only the first three actors listed, separated by semicolons)
* Ratings
* Number of Ratings

Make sure that your python script produces results that do not need additional cleanup. For example, the entry "Writer" should only contain "Stephen King; Frank Darabont", not "Stephen King (short story "Rita Hayworth and Shawshank Redemption"); Frank Darabont (screenplay)".

Your output should look like `sample_HTML.csv`.

### Hints

* Have a look at the `find()` function in the documentation of BeautifulSoup4 and look for the CSS selectors, they will make this exercise much easier! Also check out (again) the examples in the scraping assignment. 
* The script will likely be slow, but that is no problem.

## Alternative exercise

You are allowed to create a script from scratch that crawls a different datasource, but it should be of similar complexity to this exercise. This means that it should have at least 5 attributes per entry and at least 200 entries. Also, to collect all information, you should at least follow links to other webpages (do not just scrape a single webpage). If you are unsure whether your data source is complex enough, please ask in class.
