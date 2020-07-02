# Creating your own visualization

In this assignment, you'll create a visualization of the development in different parts of the world over a timespan of 60 years. You'll be using the 'gapminder' dataset that contains information about the average life expectancy and total fertility (number of children per woman) per capita of 244 countries from 1964 - 2013. From this dataset you can derive some interesting dynamics on global developments in this time period.

## Acquiring data

* You can download the data [here](Gapminder.csv).

The data set, taken from [Gapminder.org](https://www.gapminder.org/), contains information about the population size, life expectancy at birth (years), and the total fertility (children per woman) of 244 countries per year from 1964 - 2013. The region of every country (e.g. South Asia, Sub-Saharan-Africa, etc.) is also provided.

## Specification

Many medical and social advances have been made since the 60s and the goal of your visualization will be to show the effect of this in an informative way. More specifically, you would like to answer the following question: "How did life expectancy and fertility change between 1964 - 2013 for different countries and continents?".

One way to do this is by creating an interactive plot that is able to show how fertility is related to life expectancy for every country, for every year. With this visualization you can show in which countries people live long lives in small families or in contrast, short lives in large families. In addition, you might also want to add elements like population size and regions in your visualization, or even find a world map to display the data on.

Some smaller research questions you might be able to answer with this visualization are:

* Did differences between countries become smaller or larger between years?
* Which regions developed most in this time period?
* Are there countries that experienced a decrease in life expectancy at some point in time? Can you explain why this happened?

## Some readings

Before you start creating your visualization, take some time to think about how you would like to design your visualization. What exactly would you like to show and how are you going to show this? What shapes, colors, axes will you use? How do you choose the range of the axes? Below you can find some readings that might be helpful in making your decisions:

* [Visual encoding](https://www.targetprocess.com/articles/visual-encoding/), Michael Dubakov
* [Learning to see](https://ia.net/topics/learning-to-see),  Oliver Reichenstein
* [Data visualization: Clarity of Aesthetics](https://dataremixed.com/2012/05/data-visualization-clarity-or-aesthetics/) (part 1, 2 and 3), Ben Jones

## Creating your visualization

Create your own visualization in order to answer the research question. Your visualization has to meet the following criteria:

* Create the visualization using the Bokeh visualization library
* Write your code and show the visualization in a Jupyter notebook.
* Try to display all information in one figure only
* The visualization should be interactive. Users should be able to interact with the visualization in order to get more information by, for example, hovering over your figure or being able to select what part of the dataset the user would like to see (filtering), e.g. letting users choose which year they would like to see in the visualization.
* With the visualization you should be able to answer the research question above, and at least one extra (smaller) research question.
* The research questions you decided upon should be clearly stated, and answered.
* The code that is used to create the visualization should be well designed, clear, and neatly formatted. Use functions, choose useful names for your variables, prevent code repetition, place comments, etc.

## Analysis

After you've created your figure, reflect on the figure your script creates. Are all elements present in the figure? Is there something you would do different if you were going to redesign your visualization? Think about the consequences of the design choices you have made. Most important: are you able to clearly answer your research questions? Make sure to clearly state your answers in your notebook.
