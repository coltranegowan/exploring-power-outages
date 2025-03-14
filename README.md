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


### Univariate Analysis


### Bivariate Analysis


### Interesting Aggregates


## Assessment of Missingness
### NMAR Analysis


### Missingness Dependency

## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis

