# D.C Parking Tickets

## Overview

In this README you will find:
- Problem Statement
- Datasets we used
- Data Cleaning
- Data Dictionary
- Modeling info
- Conclusions

## Problem Statement

You woke up today with a smile on your face, ready to take on the day. You have all these things to get done, and are thankful that you own a car in the city so you don't have to rely on the schedule of public transportation. You arrive at your car to find a little pink slip attached to your windshield; it's a parking ticket. You pull it out and take a look; your heart sinks. We've all been there. Getting a parking ticket is a sure-fire way to bring down your mood, and potentially your bank account balance.

While there is no guaranteed approach to avoiding a parking ticket, other than not owning a car, we hope to be able to find trends that might decrease your chances of getting a parking ticket or having your fine dismissed if you do get one. Using data from Open Data DC on parking violations issued from july 2017 - july 2019 in DC, we want to develop a classification model that will predict the likelihood of having a ticket being liable or dismissed. We will gridsearch over machine learning algorithms such as KNN, Logistic Regression, and Support Vector Machines to develop the most robust model that optimizes predictive accuracy. We will also perform EDA on the categorical features in the dataset to try and unearth trends that might reduce the chances of getting a parking ticket.


## Datasets

Since our dataset is over the 100MB github file limit, we are unable to upload the data. However, here is a brief description of how we obtained our data.

Data was collected from [Open Data DC](https://opendata.dc.gov/), which was orignally sourced from Washington DC DMV. The timeframe we used was from July 2017 to July 2019. The data was archived into datasets grouped by month, so we had to individually download 24 datasets and compile them together into one giant dataset. We chose the dates we used for our data because they had the most consistent number of feature columns out of other date ranges we initially sampled. The individual datasets can be accessed from the Open Data DC site by typing 'parking violations issued in [month, year]' of interest, in the search bar.


## Data Cleaning
- Concatenated monthly datasets on parking violations over a 2 year period from july 2017 - july 2019 from Open Data DC.
- Dropped all columns with over 80% nulls, and columns that were not relevant to our analysis.
- COnverted issue_time and issue_date columns into a DateTime object and created new columns for year, month, date, weekday, hour and minute.
- Extracted quadrant data from location column to create individual one-hot encoded quadrant categories
- Then reverse one-hot encoded using .idxmax() method to get categorical column of all the quadrants.
    - Dropped rows with missing quadrant data or more than one quadrant represented.



## Data Dictionary

|Feature|Type|Dataset|Description|
|---|---|---|---| 
|objectid|int64|tickets|ID number of the ticket|
|issuing_agency_name|object|tickets|name of the agency that issued the ticket|
|issuing_agency_short|object|tickets|code for the agency that issued the ticket|
|violation_code|object|tickets|code for the violation|
|violation_proc_desc|object|tickets|description of the violation|
|location|object|tickets|address where violation occurred|
|disposition_type|object|tickets|status of the ticket|
|fine_amount|float64|tickets|dollar amount of the fine|
|total_paid|int64|tickets|dollar amount paid|
|latitude|float64|tickets|latitude of where the ticket was issued|
|longitude|float64|tickets|longitude of where the ticket was issued|
|year|int64|tickets|year 2017-2019|
|month|int64|tickets|month of the year 0-12|
|date|int64|tickets|month date|
|day|int64|tickets|day of the week 0:Sunday 1:Monday etc|
|hour|int64|tickets|hour of the day 0-23|
|minute|int64|tickets|minute 0-59|
|loc_tokens|object|tickets|indiviual words of location split into a list|
|NE|int64|tickets|northeast ward of D.C |
|NW|int64|tickets|northwest ward of D.C |
|SE|int64|tickets|southeast ward of D.C |
|SW|int64|tickets|southwest ward of D.C |
|quadrant|object|tickets|location of ticket issued|


## Exploratory Analysis
* Location Segmentation
* Time Analysis
* Exploratory Agency analysis
* Other


### Clustering  ***location Segmentation***

In order to make location segementation, we will do clustering for latitude and logitude by using k-Means method. 
We have been using both the **original** dataset contains **2,669,807** rows of data and the **cleaned** dataset which contains **896,755** rows of data.
Because the datasets are very large(2 million  or 800 thousands), we could not calculate the silouette score to decide if the model works well. By visualizing the result, we can find that the clustering gives reasonable result for every cluster number we select. For Powerpoint, we have selected the cluster number 4 and 5, and calculated the mean and sum of fine_amount.


## Conclusions

For the classification models, our goal was to maximize accuracy. The baseline accuracy score was .8199, meaning that if we always guessed the ticket was liable, we'd be right 81.99% of the time. For our logistic regression model, the score on the training set was .8193 and the score on the testing set was .8196, demonstrating low variance. For our KNN classification model, the score on the training set was .8264 and the score on the testing set was .8102, also demonstrating low variance.

95% of the tickets issued were issued by the Department of Public Works. Although dismissal rates were pretty equal among the quadrants (around 18%), the SW quadrant accounted for only 6.5% of total tickets in the dataset, whereas the NW quadrant had the most with 63.9% of total tickets issued. The top 5 most strict ticketing agencies were the United States Capitol Police, Department of Public Works, and Metropolitan Police Department Districts 7, 6, and 2. The top 5 most lenient ticketing agencies were the United States Park Police, Protective Services Department, Metro Police, Metropolitan Police Department District 1, and Special Operation Div & Traffic Div.
