
# Health code inspections

For this assignment we'll be working with health code inspection data from the
city of Seattle. It lists which restaurants where inspected when, and how
severe any health code violations were. This might range from simple things
like having thermometers installed in the fridge and labeling food containers,
to more serious concerns like seperating raw and uncooked foods, or keeping the
establishment clean of rodents and insects.

Get started by loading in the data in `Health_Code_Violations_Seattle.csv` and
take a first look at the type of data it contains. If you want to print the
data frame, make sure to only print the [head](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.head.html)
as the file is quite large. For this whole assignment, you should take some
care with what parts of the data you try to print and inspect, as is it can
take quite a while to print all of it.

We'll combine this data with restaurant reviews from *Yelp* to see if we can
find any indicators that a restaurant has serious health code violations. The
question we'll try and answer with this combined data is:

* Are there any words whose presents in a *Yelp* review might indicate there is
a serious health code violation present at the restaurant?

Given such a list of words, we could then automatically select certain
restaurants for inspection, which could help the *Washington State Department
of Health* reduce the number of serious health code violations.

## Yelp Data

The data itself is a part of the open [yelp dataset](https://www.yelp.com/dataset/documentation/main),
which is a subset of *Yelp* data that is publicly available. Take a look at the
descriptions on that page to see what the different features of this dataset
are. The goal we have here, based on our question, is to match health code
inspections with reviews. However, `review.json` only contains `business_id`'s
for each review, which we cannot directly link to the inspections. The
`business_id`'s can be found in another file though, `business.json`, that also
contains the more descriptive name and address information for the business.
Linking each review to their respective business will therefore be a good
first step.

All the Yelp data is in *JSON* format, which you might not be familair with
yet. It is a very common format on the web and is probably the second most
common format in which you will encounter data, after *CSV*. Read a short
introduction on JSON [here](https://www.w3schools.com/whatis/whatis_json.asp)
if you've never worked with it before.

### Reading the data

Python has a built-in module for processing JSON files, unsurprisingly called
`json`. You can load json objects from a string format using [loads](https://docs.python.org/3.8/library/json.html#json.loads).
The result will be some combination of dictionaries and lists, both familiar
Python datastructures, which take the place of *objects* and *arrays* in JSON
respectively.

Open the `business.json` file with a text editor and inspect what the structure
is exactly. Write some code to read this file and process the JSON objects
using `loads`. If you need a refresher how to read files in Python, take a look
[here](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files).

Thinking ahead a little, we will need to match these businesses to reviews
later on, which means searching for each business by `business_id`. Given this
dataset is really quite large, programming this search efficiently will be
crucial. Efficient repeated searches in large volumes of data should already
be ringing some datastructure bells and indeed, we'll build a dictionary (or
two) for this. 

As we'll be searching for the `business_id`'s, we'll be using those as the
dictionary **keys**. The **values** should be the JSON objects containing all
the other information about the business. Thinking ahead again, we'll want to
combine all the reviews for a business in one place, so add one more attribute
to the JSON object of each business, called `'reviews'`, which should be an
empty list (we'll fill it later).

The final structure should look something like:

    {'nxeMvqQDJ5ocA4toqRE3CQ': {'business_id': 'nxeMvqQDJ5ocA4toqRE3CQ',
                                'name': 'Ximaica',
                                ...,
                                'categories': ['Restaurants'],
                                'reviews': []},
     'oyxW3ZP60Z9b7F3LciaCjQ': {'business_id': 'oyxW3ZP60Z9b7F3LciaCjQ',
                                'name': 'XO Bistro',
                                ...,
                                'categories': ['French', 'Restaurants'],
                                'reviews': []},
     '0iyzWPc4C58sAJUAAE-BIg': {'business_id': '0iyzWPc4C58sAJUAAE-BIg',
                                'name': 'Xplosive Mobile Food Truck',
                                ...,
                                'categories': ['Food', 'Vietnamese', 'Food Trucks', 'Filipino', 'Restaurants'],
                                'reviews': []},
     ...}

But remember, *don't print the whole data frame!*


### Aside: Considerations for structuring data

Before continuing with filling this dictionary with reviews, we'll take a brief
aside to discuss different ways to represent this (or any other) data, and why
you might choose one method over the other. The main consideration here is
between representing your data in a flat table-like structure, like a pandas
*DataFrame*, or a hierarchical structure consisting of dictionaries and lists,
like the *JSON* structure. Knowing both should help you choose the structures
for you own data processing projects.

The `business.json` data seems like it is actually very close to data you might
represent in a *DataFrame*. In fact, if the `'categories'` attribute wasn't a
part of the data, it would be natural to interpret every attribute as a
column name and every new line as a row in the DataFrame. Pandas even comes
with a built-in function to convert json data to a DataFrame: [read_json](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_json.html).

So one of the problems here is that we can't easily convert the part of the
data which is hierarchical, e.g. objects or lists contained in other objects,
as there is no clear conversion to a table format. However, we could just
choose to drop the `'categories'` part of the data, as it isn't directly
relevant to our question, and then we could use some of the nice pandas
features to process the data.  Specifically, merging data from different
sources can be really easily in pandas: [merge dataframes](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html#database-style-dataframe-or-named-series-joining-merging)

Merging allows you to combine data frames based on values in a specific column,
making news rows consisting of each of the matched pairs, which would be
exactly what we want when matching the `business_id`'s of the reviews and the
business descriptions. Having a hierarchical representation has its advantages
too though, as all relevant data can be stored together. The reviews can just
be put in a list contained within each business (the empty `reviews` list you
just added), whereas in a *DataFrame* we'd have to perform `groupby` operations
every time we'd want to do something with all the reviews for a business, which
is expensive.

There are trade-offs to both structures and a valid argument could be made for
using either here. However, after this merge, we'll want to also merge in the
inspection results, which would become a very complex DataFrame with *a lot* of
duplicate information. So here we will already make the choice to stick with a
dictionary representation, to ensure the later operations will also be
managable. For now it is just useful to know both these options exist, for
when you need to merge data for you own projects.

### Merging the JSON reviews

Next, lets actually merge the reviews into the business dictionary. Read the
`review.json` file into your program, reusing some of the code from loading
`business.json`. When you've loaded a review object, search for the
corresponding business in your business dictionary and add the review to the
list of reviews there. The merged results should end up looking something like:

    {'nxeMvqQDJ5ocA4toqRE3CQ': {'business_id': 'nxeMvqQDJ5ocA4toqRE3CQ',
                                'name': 'Ximaica',
                                ...,
                                'categories': ['Restaurants'],
                                'reviews': [{'review_id': 'XmAqfWFa_GyBj5nmU_ETXA',
                                             'business_id': 'nxeMvqQDJ5ocA4toqRE3CQ',
                                             ...,
                                             'stars': 3.0,
                                             'text': "Jamaican themed bar. DJ spins reggae ..."},
                                            {'review_id': 'xkSh71JlbLm9K9trwPJd7A',
                                             'business_id': 'nxeMvqQDJ5ocA4toqRE3CQ',
                                             ...,
                                             'stars': 2.0,
                                             'text': 'This place is closed. The bar there ..."},
                                            {'review_id': '7qlD758wI3g4vK8RETGebQ',
                                             'business_id': 'nxeMvqQDJ5ocA4toqRE3CQ',
                                             'stars': 5.0,
                                             'text': "Sucks that this place is closed ..."},
                                            ...]},
     'oyxW3ZP60Z9b7F3LciaCjQ': {'business_id': 'oyxW3ZP60Z9b7F3LciaCjQ',
                                'name': 'XO Bistro',
                                ...,
                                'categories': ['French', 'Restaurants'],
                                'reviews': [{'review_id': 'taAal2VzHOXJHK4w0APmYQ',
                                             'business_id': 'oyxW3ZP60Z9b7F3LciaCjQ',
                                             ...,
                                             'stars': 5.0,
                                             'text': "Lovely food and top-notch service ..."},
                                            {'review_id': 'TtoNrYmmGWqZ4RGDEv0MNA',
                                             'business_id': 'oyxW3ZP60Z9b7F3LciaCjQ',
                                             ...,
                                             'stars': 4.0,
                                             'text': "I visited this place a few times ..."},
                                            {'review_id': '7Ni9DMjJ3zZ9kq2jARmmyQ',
                                             'business_id': 'oyxW3ZP60Z9b7F3LciaCjQ',
                                             ...,
                                             'stars': 5.0,
                                             'text': "Chalk one up for this low-key, ..."},
                                            ...]},
    ...}


## Matching the inspections with reviews

Next we'll get started with the harder part of combining this data, merging the
inspection results into this same structure. Take a quick look at the file
containing all the inspections, which you already loaded at the start of this
assignment. The most logical way to match these inspection results to the
reviews is just using the name of the business, although we could also include
information like address for a more accurate match.

The business names in the inspection results look like they are formatted a
little differently, so we'll need to do some conversions to ensure they match
the names used in the business descriptions. The names appear to be in
uppercase, so that'll definitely be something to fix. Also, it is probably a
good idea to remove any punctuation marks, as those are very likely to be used
inconsistently in different data sources. Write a function called `clean_name`,
which takes a restaurant name, converts it to all lowercase and removes any
punctuation marks.

When merging in the inspection results with the review data, we'll be
repeatedly be searching for a restaurant with a specific name, and not its
`business_id`. So while doing these conversions, it would also be a good idea
to restructure the dictionary containing the data so we can easily search based
on the name of the restaurant. The values of this new dictionary should be the
same as in the old dictionary, but the keys should the cleaned restaurant
names, instead of the id's. Also, to clean up the data a little more, remove
all restaurants that don't have any reviews, as they won't help answer our
question. Finally, add a new attribute `'inspections'`, containing an empty
list, in each object, just like you did for the reviews, where all the
inspection results can be merged into.

Making a whole new dictionary with different keys might seems inefficient, but
given we will have to try and find every business by name, it is actually much
faster. This approach will be 2 $$O(N)$$ loops; one to rebuild the dictionary
and one to search for every name, as one search itself will now be $$O(1)$$ in
the new dictionary.  Compare that to the version using nested loops to match
the names, which would be $$O(N^2)$$, so *extremely* slow for data this size.
This transformation is therefore part restructuring (changing the keys by which
the dictionary is indexed) and part content modification (converting the
restaurant names to the cleaned format), with the sole aim of reducing the
complexity of the next transformation.

The restructured dictionary should end up looking something like:

    {'youngs restaurant': {'business_id': 'IbRT1K1TQ7mFXjlIZ9onUw',
                           'name': "Young's Restaurant",
                           ...,
                           'categories':  ['American (Traditional)', 'Chinese', 'Restaurants'],
                           'reviews':     [{'review_id': 'K90lbzJj_HUVxqkhp87bIA',
                                            'business_id': 'IbRT1K1TQ7mFXjlIZ9onUw',
                                            ...,
                                            'stars': 5.0,
                                            'text': "One of my favorite breakfast/brunch ..."},
                                           {'review_id': 'qTcdlvOHiI84_5e6mfe2QQ',
                                            'business_id': 'IbRT1K1TQ7mFXjlIZ9onUw',
                                            ...,
                                            'stars': 3.0,
                                            'text': "Honestly I wanted to try this place ..."},
                                           ...],
                           'inspections': []},
     'yu shan':           {'business_id': '0rqp9CbHsZZEw1UOUHInWg',
                           'name': 'Yu Shan',
                           ...,
                           'categories':  ['Chinese', 'Restaurants'],
                           'reviews':     [{'review_id': 'KOwWTvfTcl7jCbu8xoWxmw',
                                            'business_id': '0rqp9CbHsZZEw1UOUHInWg',
                                            ...,
                                            'stars': 5.0,
                                            'text': "This five star is for in house only ..."},
                                           {'review_id': '1cQGC96CIWC3HhULg_F6Dw',
                                            'business_id': '0rqp9CbHsZZEw1UOUHInWg',
                                            ...,
                                            'stars': 1.0,
                                            'text': "I ordered this food for delivery ..."},
                                           ...],
                           'inspections': []},
     ...}


### Merging in the inspections

Now apply the same `clean_name()` function to every name in the inspections
DataFrame. Use [map](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.map.html)
to apply the function to entire `'Name'` column at once and store the result in
that same column. Using the `clean_name()` function to clean up both sources
of data should ensure consistent results for this transformation.

Loop over the inspections DataFrame using [iterrows](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iterrows.html)
and search for the (cleaned) name in your restaurant dictionary. Add the
inspection to the list of inspections for that restaurant. Transform the row
*Series* to a dictionary using [to_dict](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.to_dict.html),
to make the data consistent in type.
    
The final result of combining all these data files in single structure should
look something like:

    {'youngs restaurant': {'business_id': 'IbRT1K1TQ7mFXjlIZ9onUw',
                           'name': "Young's Restaurant",
                           ...,
                           'categories':  ['American (Traditional)', 'Chinese', 'Restaurants'],
                           'reviews':     [{'review_id': 'K90lbzJj_HUVxqkhp87bIA',
                                            'business_id': 'IbRT1K1TQ7mFXjlIZ9onUw',
                                            ...,
                                            'stars': 5.0,
                                            'text': "One of my favorite breakfast/brunch ..."},
                                           {'review_id': 'qTcdlvOHiI84_5e6mfe2QQ',
                                            'business_id': 'IbRT1K1TQ7mFXjlIZ9onUw',
                                            ...,
                                            'stars': 3.0,
                                            'text': "Honestly I wanted to try this place ..."},
                                           ...],
                           'inspections': [{'Name': 'youngs restaurant',
                                            'Program Identifier': "YOUNG'S RESTAURANT",
                                            ...,
                                            'Violation_Record_ID': nan,
                                            'Grade': 1.0},
                                           {'Name': 'youngs restaurant',
                                            'Program Identifier': "YOUNG'S RESTAURANT",
                                            ...,
                                            'Violation_Record_ID': 'IVKWHBZBH',
                                            'Grade': 1.0},
                                           ...]},
     'yu shan':           {'business_id': '0rqp9CbHsZZEw1UOUHInWg',
                           'name': 'Yu Shan',
                           ...,
                           'categories':  ['Chinese', 'Restaurants'],
                           'reviews':     [{'review_id': 'KOwWTvfTcl7jCbu8xoWxmw',
                                            'business_id': '0rqp9CbHsZZEw1UOUHInWg',
                                            ...,
                                            'stars': 5.0,
                                            'text': "This five star is for in house only ..."},
                                           {'review_id': '1cQGC96CIWC3HhULg_F6Dw',
                                            'business_id': '0rqp9CbHsZZEw1UOUHInWg',
                                            ...,
                                            'stars': 1.0,
                                            'text': "I ordered this food for delivery ..."},
                                           ...],
                           'inspections': [{'Name': 'yu shan',
                                            'Program Identifier': 'YU SHAN',
                                            ...,
                                            'Violation_Record_ID': nan,
                                            'Grade': 1.0},
                                           {'Name': 'yu shan',
                                            'Program Identifier': 'YU SHAN',
                                            ...,
                                            'Violation_Record_ID': 'IV53U6A9T',
                                            'Grade': 1.0},
                                           ...]},
     ...}

