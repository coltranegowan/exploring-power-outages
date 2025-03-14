# Exploring Power Outages
Coltrane Gowan



## Introduction
This project analyzes the dataset Power Outages from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, which contains data on major power outage events in the continental U.S. from January 2000 to July 2016. All outages part of this dataset affected at least 50,000 customers or caused an energy loss of 300 MegaWatts or more. Information on each outage includes the geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages.

This dataset contains 57 columns and 1534 rows, but for the purposes of this project, these are the columns I will focus on:
| Column Name | Information |
| ----------- | ----------- |
| 'YEAR' | Indicates the year when the outage event occurred |
| 'MONTH' | Indicates the month when the outage event occurred |
| 'U.S._STATE' | Represents all the states in the continental U.S. |
| 'CLIMATE.REGION' | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.) |
| 'ANOMALY.LEVEL' | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W) |
| 'CLIMATE.CATEGORY' | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI) |
| 'OUTAGE.START.DATE' | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region) |
| 'OUTAGE.START.TIME' | This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region) |
| 'OUTAGE.RESTORATION.DATE' | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| 'OUTAGE.RESTORATION.TIME' | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| 'CAUSE.CATEGORY' | Categories of all the events causing the major power outages |



The question I want to center this exploration on is:
How have characteristics of major power outages changed over time, and specifically how has the changing climate influenced how the distribution of these outages?

In researching this question, we can better understand the role of the climate in our energy grid, and determine whether more intense precautions need to be put into place to curb the frequency of these events.


## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
First, I need to clean the dataframe to prepare it for the upcoming analysis.

- I only want to keep the columns listed above: 'YEAR', 'MONTH', 'U.S._STATE', 'CLIMATE.REGION', 'ANOMALY.LEVEL', 'CLIMATE.CATEGORY', 'OUTAGE.START.DATE', 
    'OUTAGE.START.TIME', 'OUTAGE.RESTORATION.DATE', 'OUTAGE.RESTORATION.TIME', and 'CAUSE.CATEGORY'. All other columns are dropped. This way, I do not waste space and computing time with columns that are not relevant.

- Then, I combine the 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' columns into one 'OUTAGE.START' column in datetime format for efficiency.

- I do the same for the 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' columns, combining them into 'OUTAGE.RESTORATION' in datetime format.

- After, I drop the old columns '.DATE' and '.TIME' respectively, since they are no longer needed.


The first couple rows of the cleaned dataframe are shown below:

|   OBS |   YEAR |   MONTH | U.S._STATE   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION | SEASON   |
|------:|-------:|--------:|:-------------|:-------------------|----------------:|:-------------------|:-------------------|:--------------------|:---------------------|------------------:|:---------|
|     1 |   2011 |       7 | Minnesota    | East North Central |            -0.3 | normal             | severe weather     | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |        51         | Summer   |
|     2 |   2014 |       5 | Minnesota    | East North Central |            -0.1 | normal             | intentional attack | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |         0.0166667 | Spring   |
|     3 |   2010 |      10 | Minnesota    | East North Central |            -1.5 | cold               | severe weather     | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |        50         | Fall     |
|     4 |   2012 |       6 | Minnesota    | East North Central |            -0.1 | normal             | severe weather     | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |        42.5       | Summer   |
|     5 |   2015 |       7 | Minnesota    | East North Central |             1.2 | warm               | severe weather     | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |        29         | Summer   |


### Univariate Analysis
To start off, I explore some univariate data to identify patterns in the data.

I first wanted to visualize the amount of outages per year to identify potential outliers:

<iframe
  src="assets/outages_per_year.html"
  width=800
  height=600
  frameborder=0
></iframe>


I then looked at the amount of outages by cause in the dataset overall:

<iframe
  src="assets/outages_by_cause.html"
  width=800
  height=600
  frameborder=0
></iframe>

### Bivariate Analysis
Next, I look at the possible relationships between columns, which will help guide the direction in which I will explore this data. Here are a couple I found relevant to my question.

Here I looked at the the amount of outages per year and cause combined, to get a sense of the distribution of the data as a whole:

<iframe
  src="assets/outages_by_year_cause.html"
  width=800
  height=600
  frameborder=0
></iframe>


I also singled out the amount of severe weather outages in the dataset to see if there was a clear trend worth diving in to, but there was no clear trend in the data:

<iframe
  src="assets/severe_outages.html"
  width=800
  height=600
  frameborder=0
></iframe>

### Interesting Aggregates
I then made several pivot tables, grouping by different statistics that I thought would be telling in understanding the distribution of the data. In the dataframe below, I grouped by 'CLIMATE.CATEGORY' and 'CAUSE.CATEGORY', seeing if there was a meaningful difference in distribution based on the climate:

'| CLIMATE.CATEGORY   |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |\n|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|\n| cold               |                  19 |                      19 |                  122 |          15 |              22 |              239 |                              37 |\n| normal             |                  28 |                      26 |                  226 |          17 |              34 |              354 |                              59 |\n| warm               |                  10 |                       5 |                   70 |          14 |              13 |              166 |                              30 |'

## Assessment of Missingness
### NMAR Analysis


### Missingness Dependency

## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

