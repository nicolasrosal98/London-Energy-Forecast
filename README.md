# London Energy Forecast

Using an LSTM model to forecast energy consumption in London (analysis by Acorns included).

## Datasets

The datasets required can be found in the following [Kaggle link](https://www.kaggle.com/jeanmidev/smart-meters-in-london)

<br />


## Purpose

The purpose of this project was to build a deep learning model capable of recognizing trends in energy consumption and generating forecasts of future usage levels. All this, to help:

* Energy Companies understand user consumption trends and better generate consumption estimates in the future.
* Users by providing information about their future energy consumption levels (one month ahead). 
* The local government better understand the overall energy consumption patterns and future behaviour in London. 
* Any interested party by providing a framework that allows them to further explore how acorns, contract type, seasons, etc. (other features) can be useful at generating more accurate forecasting models. 

<br />

 
## Useful Links 

Acorn User Guide to understand consumer classification: [guide](https://acorn.caci.co.uk/downloads/Acorn-User-guide.pdf)

<br />

## Methodology

### 1. Cleaning and Feature Engineering

The goal of this section was to clean and combine all available datasets into a single dataset that would be easier to handle for exploratory data analysis. Moreover, during the process, the following features were engineered to further explore energy consumption:

* Segmentation of Average Energy Consumption by Parts of Day -- It was hypothesized that average energy consumption could be divided into different parts of the day, namely morning, afternoon, evening, etc. To test this hypothesis, halfhourly average consumptions vs. time was plotted. As a result, four regions where consumption trends shifted were identified. These regions were used to segment the data into Dawn, Morning-Afternoon, Evening and Night parts of day (according to [Merriam-Webster's division](https://www.learnersdictionary.com/qa/parts-of-the-day-early-morning-late-morning-etc).<br />
![Average Consumption by Part of Day](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/averagepartsofday.png)
<br />

* Holiday Binary Feature -- The information contained in the holiday dataset was used to create a binary column that identified which days were holidays (1) and which were not (0). 
<br />

* Season Clustering -- It was hypothesized that energy consumption would vary depending on seasons. Hence, the weather data was clustered using FAMD (Factor Analysis for Mixed Data) components and a K-Means algorithm. Originally, it was planned to use PCA analysis but, given the data presence of different data types (numerical and categorical features), a FAMD analysis was required. The final clusters were not defined by strict season divisions given that end of spring - start of autumn and start of spring - end of autumn would present similar weather conditions. For this reason, the data was grouped by 'winter', 'summer', 'StartSpr/EndAut', and 'EndSpr/StartAut'. <br />

NOTE: After the 'Clustering for Feature Engineering Section', a 3D plot of the cluster could not be visualised in the jupyter notebooks uploaded. This is that plot: <br />
![3D Clustering Plot](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/cluster.png)
<br />


### 2. Exploratory Data Analysis

The goal of section was to further explore the data set created after 'Cleaning and Feature Engineering' and prepare the data set for the 'Modelling' section. 

* Plotting Distributions -- After plotting the distributions of each variable, it was identified that energy-related features (e.g. energy_sum, Avg.Dawn, etc.) and visibility presented a highly skewed distribution. This gave evidence of the possible presence of outliers in the data.
<br />

* Outliers Analysis -- It was verified that 'visibility', 'energy_sum', 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening', and 'Avg.Night' presented outliers. Thus, this would be explored to understand whether the outliers would be reasonable given the scale and nature of the data. In this case, it was evidenced that most of the outliers occur in 'Winter' and 'StartSpr/EndAut' which makes makes sense as visibility decreases during colder periods. Similarly, it has been found that households tend to consume more energy during colder climates. Hence, 'visibility', 'energy_sum', 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening', and 'Avg.Night' outliers will be kept. <br />
![Visibility Outliers vs. Season](image1.png "title-1") ![Energy Sum Outliers vs. Season](image2.png "title-2")

*  -- 

<br /><br />

### 3. Modelling

<br /><br />

## Conclusions

<br /><br />

## Limitations and Suggestions

<br /><br />

## Sources

<br /><br />
