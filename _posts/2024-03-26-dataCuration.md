---
layout: post
title:  "Compiling LDS Temple Data"
author: James Christensen
description: Using Pandas to scrape and combine datasets from <a href="churchofjesuschristtemples.org" target="_blank">churchofjesuschristtemples.org</a>
image: /assets/images/logo.png
---

*The dataset and its Github repo is linked at the very bottom of this blog post*
## Introduction
Since its near inception, the Church of Jesus Christ of Latter-Day Saints has been building temples. Temples are buildings that they
view as holy and as the only place where certain worship practices can be carried out. Their beautiful and unique architecture have been inspiring
and awe inducing.

Information in regard to specific temples can be hard to come by, if one were to use only official church resources.  Luckily, Rick Satterfield, a
faithful member of the Church of Pocatello Idaho, has been upkeeping a website about the temples since 1998. To this day, news in regard to temples
can scarcely last 45 minutes before being reported on by Satterfield. His efforts have made him the upmost authority in the world on Temples of the Church

<img src="{{site.url}}/{{site.baseurl}}/assets/images/144.jpg" alt=""/>

Luckily for us, a lot of the data in regard to the temples has been compiled and is easy to scrape from his website. Data ranging from the elevation of
where each temple was built, how far each one is from the nearest one, how large each temple and its plot is, their locations, etc. With a multitude
of information, it would be interesting to cross reference these data sets and compare the information in them. 

I personally, am interested in investingating whether the Church builds larger temples in places of higher elevation. This seems to be intuitive
as many members of said church live in the in the intermountain west (the states of Idaho, Utah, Nevada, and Arizona. Parts of other western
states are also included) Luckily, Pandas allows us to investigate just that very easily.

## Making the Dataset
### Step 1: Installing and Importing the Necessary Libraries
To start, it is important to make sure you have all of the dependencies installed. For purposes of this tutorial (and for data science uses more generally)
you will want to install the following dependencies:
* numpy
* re
* pandas

These libraries have many more uses than what we will be using them for today. If you dont have one or more of these dependencies installed, the University
of Michigan has released and excellent tutorial on multiple ways to do this. <a href="https://docs.support.arc.umich.edu/python/pkgs_envs/" target="_blank">Click here to access that tutorial</a>
Once this has been done, we can now begin importing these into python. This can be done with the following code:

```{python}
import pandas as pd
import numpy as np
import re
```

If you encounter any errors or issues with importing these libraries, please look above at the University of Michigan tutorial and attempt to redownload 
each of the dependencies.

### Step 2: Creating the Sub-data Sets
Before doing this, it is important to note that I believe that using this data is completely ethical. Rick has made this data publicly available as
as service to the public. He makes zero money, receives zero financial support, and does this completely out of a love and devotion to temples. From
what I can surmise from his dealings online, it appears that not only would he be ok with investigating the data in this way, he would encourage it.

The first thing we need to do is pull the two respective datasets. Once these are imported, we will move towards merging these datasets together.

#### Temple Dimension Dataset
Rick has made this dataset available on the statistics part of his webpage. The data is entered in as an html table. Pandas makes it super easy to
pull this type of data using the read_html function. This function will return a python list of all of the tables on the page. Hence the subsetting 
to the very first table. 
```{python}
urlDimensions = 'https://churchofjesuschristtemples.org/statistics/dimensions/'

dimensionTable = pd.read_html(urlDimensions)[0]
dimensionTable
```
These are the results from the dimension dataset:
<img src="{{site.url}}/{{site.baseurl}}/assets/images/dimen.png" alt=""/>

#### Temple Elevation Dataset
The elevation dataset can be pulled in the same exact way.
```{python}
urlElevation = 'https://churchofjesuschristtemples.org/statistics/elevations/'

elevationTable = pd.read_html(urlElevation)[0]
elevationTable
```
These are the results from the elevation dataset:
<img src="{{site.url}}/{{site.baseurl}}/assets/images/elev.png" alt=""/>

### Step 3: Merging the Datasets
Now that we have the two datasets, we can move to merging them. In order to join them, we have to specify a column that they have in common. In our
case, this is the name column. 
The Church of Jesus Christ of Latter-Day Saints often releases the site location before they release other information about the temple. This 
includes information such as the size, rendering of the temple, etc. Because of this, the elevation data set is much larger as information for 
temples that are under construction or soon to be, have their location and as such their elevation released early.

Because of this, it seems appropriate to omit the temples which are not included in the dimensions dataset. As the absence of information removes
their usefulness for the sake of this analysis.

They can be merged easily doing the following:
```{python}
finalDataset = pd.merge(dimensionTable, elevationTable, on='Temple', how='left')
finalDataset
```
The 'on' parameter is the column which the two tables have in common. The 'how' specifies the direction of the join. <a href = "https://www.w3schools.com/sql/sql_join.asp" target="_blank">W3 Schools</a> has excellent explanations and tutorials on using each type of join.

These are the results from the merged dataset:
<img src="{{site.url}}/{{site.baseurl}}/assets/images/mergeSet.png" alt=""/>

### Step 4: Cleaning the Dataset
In a lot of cases, the dataset that we find on the internet won't be completely clean. There are many things that could be going on, but in this case
all of the columns are strings. This leads to the need to remove the commas from the numbers, converting the columns to integer or float values, among
other things. 

On my repository, I go into large detail with what I did to and how I cleaned the data. The result from these efforts was the following dataset:
<img src="{{site.url}}/{{site.baseurl}}/assets/images/final.png" alt=""/>

## Conclusion
The point of this post was to outline how and what I did to compile the dataset. I look forward to my next post when we can start investigating the
major question of whether there is a quantifiable relationship between elevation and the size of a temple. I also will be exploring other relationships
between the data including their plot size and locations.

If any of you have any suggestions to things I could improve or some trends you would like to see explored. Please feel free to reach out to me! 

Here is the github repo of the dataset as promised. It contains the comma separated value version of the dataset, the markdown detailing out the 
files in the repo, and the notebook with documentation and all the code for how this data set was curated.
<a href = "https://github.com/jmc8290/templesDataSet" target="_blank">Click here for the Github Repo!</a>