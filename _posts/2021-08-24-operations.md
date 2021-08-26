---
layout: post
title:  "Operations"
date:   2021-08-24 20:00:20 +0900
categories: update
---

## Problem statement
For a website that acts as a data aggregator of covid data to work, there are a few steps that need to be followed:
1. Track state bulletins on a daily basis.
2. Convert the different state bulletins into a standardised format.
3. Make these entries into a data store (a database or some kind of file et al).
4. Pull data from this data store for either APIs or a website.

We follow the below steps to achieve the above mentioned requirements:
1. We track all state bulletins using a telegram bot that tracks twitter/website updates for each state and posts in a telegram channel. This is used by volunteers when they want to update data.
2. The cases format for each district per day is standardised as delta numbers for that day per category (Confirmed/Recovered/Deceased/Migrated).
3. Our data store is based out of Google Sheets that sync to a [Github repository using Github Actions](https://github.com/covid19india/data/blob/60af6e683fd22ce2f0435b7a129260aa944ea369/.github/workflows/javascript.yml#L43).
4. The Github repository is what acts like our [API endpoint]((https://github.com/covid19india/data/blob/main/src/parser_v4.py)) and a place from which our website pulls the data.

**This method is cost effective (zero cost since everything we use is a free platform). However, there are much simpler ways of doing this. Please consider using the sources of data we have and building your own tracker using these sources.**


## Detailed Explanation
# Sources for states

Please refer [this link](https://blog.covid19india.org/2020/06/15/hornbill/) for all the sources we use for data. This contains the list of sources per state and also sources we track for vaccination (cowin API).

# Mode of data input
Data operations team is responsible for updating data the data onto the Google sheets which in turn acts as the entry point for all data that we expose. Different datasets have different sheets associated with it. [This document](https://docs.google.com/spreadsheets/d/1foGJ_FwHoDnVUI7VqN-YrwrPo0A4pfLV8jFYxn4rLaw/edit#gid=0) is one of our versions that has multiple sheets corresponding to different datasets.  
The standard format for the cases sheet is shown below:
![Google Sheet](/assets/images/raw.png)

- Data is always added in delta increments. i.e, the current day's numbers for confirmed, recovered, deceased and migrated is added as one row per district in the cases sheet. 
- To calculate the delta for a given day, we use the following logic:  
```(cumulative numbers for a category for a district in today's bulletin - cumulative numbers for a category for the district taken from covid19india.org website)```
- For vaccine and testing data, the data is pulled from the sources (bulletins, cowin API) and is directly entered to a different Google sheet. There's no other calculation required for it.

# Sheets used
Below are some of the major sheets used and how they relate to the overall structure of data:  
- Raw_data: This is a sheet that has the basic district level Confirmed, Recovered, Deceased and Migrated delta per day details. This is versioned after around 20k rows to prevent it from becoming very slow.
- Statewise: This is a sheet that is an aggregated value per state. This is pulled from raw_data sheet using formulae.
- Districtwise: This is a sheet that has district level aggregates (cumulative and delta).
- Statewise/districtwise_Previous_Partition: These sheets allow us to keep a track of the values when the new version of raw sheet was created. More of this is [documented here](https://blog.covid19india.org/2020/06/07/shifttonewversion/).

# Data sync
- The data from the Google Sheets is directly pulled to a [csv file](https://github.com/covid19india/data/blob/main/src/sheets-to-csv.js). 
- These files in turn are consumed by a [json parser](https://github.com/covid19india/data/blob/main/src/parser_v4.py) that gives us the data in a format that we need for the website.
- Some of the sheets are directly synced and kept as csv files. These are directly exposed as csv endpoints.

# 


