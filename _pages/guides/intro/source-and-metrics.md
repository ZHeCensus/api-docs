---
title: Source and metrics
description: This is a guides page.
permalink: /guides/intro/2/

layout: guide
sidenav: guides-intro
---

To first get data from the Census Data API we have to know three things

- A source - what survey, and year do we want (e.g., ACS 2015 5-year estimate)
- A metric - what column we want to get the data for (e.g. population density)
- A geographic entity - (e.g state, county, census tract, census block)

Then it is put into a query object. (ex. get the population count from ACS 2017 1-year estimate for all states)

```js
{
    "sourcePath" : ["acs","acs1"],  // source (survey, ACS 1-year estimate)
    "vintage" : 2017,               // source (year, 2017)
    "values" : ["B00001_001E"],     // metric (column for population count)
    "geoHierarchy" : {              // geographic entity (grouped by state)
      "state" : "*"
    }
}
```

Each dataset provides information for different topics, years, and at different levels (national, state, county, block levels). Census Data API provides various datasets, you can explore them in detail [here](https://api.census.gov/data.html). 

We will focus on the the American Community Survey (ACS) because of its wide variability of population questions and frequent updates. To discover sources and metrics we can use the search feature in [data.census.gov](http://data.census.gov). The Slack and Gitter developer communities; and data experts are also a great source, if you can't find the data.

For the first example we will get the Population of the all the States. At [data.census.gov](http://data.census.gov) click on the **Population** under the **Subjects** heading on the main page.  You can see the various tables that have population information, select the first one "Total Population in United States"  

![](/assets/images/getting1.png)

It will bring you to this window where you can look at the table in detail. Take a note of the follow.

- **Survey/Program:** ACS
- **Year:** 2017
- **Estimate:** 1-year
- **TableID:** DP05

This will help you find the specific column ids needed for the query. For this example we will get the total population, male and female population columns. 

![](/assets/images/getting2.png)

1. Locate the Source on [https://www.census.gov/data/developers/data-sets.html](https://www.census.gov/data/developers/data-sets.html) , using the Survey/Program ID, Year, and Estimate. 

![](/assets/images/getting3.png)

2. Select the year 2017 

![](/assets/images/getting4.png)

3. Select the group, which is the prefix of the table id (DP). Note each group has different prefixes look at the example Example call for clues on the prefix

![](/assets/images/getting5.png)

4. Find your columns by searching the page (Ctrl + F) for your Tableid (DP05)

![](/assets/images/getting6.png)

We found them!! Now copy the column names (DP05_0001E, DP05_0002E, DP05_0003E) and the url 

![](/assets/images/getting7.png)

(an additional example, in a drop down)

### Constructing the Query

Following the template of a query we can fill in the following

```js
{
  "sourcePath" : [""] // source (survey)
  "vintage" : // source (year)
  "values" : [],   // metric (columns)
  "geoHierarchy" : {  // geographic entity (grouped by state)
    "state" : "*"
    }
}
```

We can find sourcePath and vintage by breaking down the url. The sourcePath is an array of the path.

> https://api.census.gov/data/2017/acs/acs1/profile/variables.html

```js
{
    "sourcePath" : ["acs","acs1","profile"],  // source (survey, ACS 1-year profile estimate)
    "vintage" : 2017,               // source (year, 2017)
}
```

Add you the column names in array, add "NAME" column too

```js
{
    "sourcePath" : ["acs","acs1","profile"], 
    "vintage" : 2017,               
    "values" : ["NAME","DP05_0001E","DP05_0002E", "DP05_0003E"], // metric (column for total count, male, and female popluation)
}
```

after your query is done, you will get this.

```js
{
    "sourcePath" : ["acs","acs1","profile"],  // source (survey, ACS 1-year profile estimate)
    "vintage" : 2017,               // source (year, 2017)
    "values" : ["NAME","DP05_0001E","DP05_0002E", "DP05_0003E"],     // metric (column for total count, male, and female popluation)
    "geoHierarchy" : {              // geographic entity (grouped by state)
      "state" : "*" 
    }
}

//result
[{
    "NAME": "Alabama",
    "DP05_0001E": 4874747,
    "DP05_0002E": 2359896,
    "DP05_0003E": 2514851,
    "state": "01"
  },
  {
    "NAME": "Alaska",
    "DP05_0001E": 739795,
    "DP05_0002E": 385776,
    "DP05_0003E": 354019,
    "state": "02"
},
  ...
}]
```

(query for the additional example)

![](/assets/images/getting8.png)

[https://api.census.gov/data/2017/acs/acs1/subject/variables.html](https://api.census.gov/data/2017/acs/acs1/subject/variables.html)

![](/assets/images/getting9.png)

"S2801_C01_001E","S2801_C01_002E" 

```js
{
  "sourcePath" : ["acs","acs1","subject "], // source (survey, ACS 1-year subject estimate)
  "vintage" : 2017,                         // source (year, 2017)
  "values" : ["NAME","S2801_C01_001E","S2801_C01_002E" ], // metric (column for total households, 
                                                          // one or more types of computing devices)
  "geoHierarchy" : {                              // geographic entity (grouped by state)
    "state" : "*" 
  }
}
```
