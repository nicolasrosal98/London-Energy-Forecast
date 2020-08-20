# London Energy Forecast

Using an LSTM model to forecast energy consumption in London (analysis by Acorns included).

## Datasets

The datasets required can be found in the following [Kaggle link](https://www.kaggle.com/jeanmidev/smart-meters-in-london)

<br /><br />


## Purpose

The purpose of this project was to build a deep learning model capable of recognizing trends in energy consumption and generating forecasts of future usage levels. All this, to help:

* Energy Companies understand user consumption trends and better generate consumption estimates in the future.
* Users by providing information about their future energy consumption levels (one month ahead). 
* The local government better understand the overall energy consumption patterns and future behaviour in London. 
* Any interested party by providing a framework that allows them to further explore how acorns, contract type, seasons, etc. (other features) can be useful at generating more accurate forecasting models. 

<br /><br />

 
## Useful Links 

Acorn User Guide to understand consumer classification: [guide](https://acorn.caci.co.uk/downloads/Acorn-User-guide.pdf)
<br /><br />

## Methodology

### 1. Cleaning and Feature Engineering

The goal of this section was to clean and combine all available datasets into a single dataset that would be easier to handle for exploratory data analysis. Moreover, during the process, the following features were engineered to further explore energy consumption:

* Segmentation of Average Energy Consumption by Parts of Day -- We hypothesized that average energy consumption could be divided into different parts of the day, namely morning, afternoon, evening, etc. To test our hypothesis, we plotted halfhourly average consumptions vs. time. As a result, we identified four regions where consumption trends shifted. These regions were used to segment the data into Dawn, Morning-Afternoon, Evening and Night parts of day (according to [Merriam-Webster's division](https://www.learnersdictionary.com/qa/parts-of-the-day-early-morning-late-morning-etc))
* asdasdasdasdasd
*


NOTE: After the 'Clustering for Feature Engineering Section', we have created a 3D plot of the cluster that could not be visualised in the jupyter notebooks uploaded. This is that plot:

![3D Clustering Plot](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/newplot.png)



### 2. Exploratory Data Analysis

<br /><br />

### 3. Modelling

<br /><br />

## Conclusions

<br /><br />

## Limitations and Suggestions

<br /><br />

## Sources

<br /><br />
