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

* Segmentation of Average Energy Consumption by Parts of Day -- It was hypothesized that average energy consumption could be divided into different parts of the day, namely morning, afternoon, evening, etc. To test this hypothesis, halfhourly average consumptions vs. time was plotted. As a result, four regions where consumption trends shifted were identified. These regions were used to segment the data into Dawn, Morning-Afternoon, Evening and Night parts of day according to [Merriam-Webster's division](https://www.learnersdictionary.com/qa/parts-of-the-day-early-morning-late-morning-etc).<br />
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
<img src="https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/visibility.png" width="450"/> <img src="https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energy_sumout.png" width="450"/>

* Correlation Analysis -- As a result of the analysis several variables such as 'temperatureLow' and 'temperatureHigh' were eliminated to reduce high dimensionality and prevent high correlation problems. Similarly, a very strong correlation between 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening','Avg.Night', and 'energy_sum' was identified. Nevertheless, all these features were kept for modelling but carefully considered in order to avoid undesired outcomes due to the correlation. All other features presented a low correlation with 'energy_sum', leading to the assumptions that they will have minimum impact on the models. For this reason, a feature importance analysis was performed later. <br />
![Heatmap](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/heatmap.png)

* Energy Sum Analysis Over Time -- Plotting energy consumption over time helped to understand how consumption varies throughout the years (months/seasons). Further analysis was made with the different energy sum averages per part of day. <br /> 
![Energy Sum over Time](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energyvstime.png)

* Energy Sum per Type of Contract -- Plotting energy consumptions per contract type allows to see differences in behaviour between different type of clients.<br /> 
![Energy Sum vs. Contract Type](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/ContractType.png)

* Energy Sum per Acorn -- Plotting energy consumption per Acorn helped analyse whether belonging to different acorn groups would affect consumption patters and behaviours. <br /> 
![Energy Sum vs. Acorn](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/acorns.png)

* Feature Importance -- To reduce the complexity of the data, a Random Forest's feature importance attribute was used to discover the features that would be important to add in the final dataset. Due to poor visualisations, a picture of the feature importance bar plot created is included below. In here, 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening', and 'Avg.Night' were identified as the most important features. This finding, together with those of the heatmap correlation analysis, led to the decision of selecting only these four features for the modelling section. <br /> 
![Feature Importance](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/featureimportance.png)

<br /><br />

### 3. Modelling

The goal of this section was to build an LSTM model capable of fitting energy trends and forecasting energy consumption.

* Why LSTM? -- Due to the nature of the problem, a model that was capable of dealing with time-series data was necessary. In this case, LSTM was chosen over GRU due to its capacity to remember longer sequences that would be needed to compute more accurate forecasts. <br /> 

* Why different architectures? -- It was found that a TimeDistributed output layer reduced significantly the MAE of forecasting tasks. Thus, this model was created with two different output layers that could change to fit the assigment to be performed (Trend recognition or Forecasting).<br /> 

* Tuning (Scores) -- The model was tuned by changing the number of LSTM layers and the number of LSTM units per layer, using dropout, l1, l2, and batch normalization regularizations, and modifying the optimizer and output layer. The best performing architecture consisted of five LSTM layers with 64 Units, Dropout of 25%, one dense time distributed output layer, compiled with a RMSProp optimizer.<br /> 


#### Conclusions

* overall energy model
  trend test MAE - 0.059765469282865524 (PICTURE)
  forecast test MAE - 0.41751142945194003 (PICTURE)
  
* energy model by acorn
  forecast test MAE average - 0.6862249281035474 (PICTURE)
  
* combination 
  forecast test MAE overall Energy - 0.43439047026906075 (PICTURE)
  forecast test MAE acorn - 0.6315548906462493 (PICTURE)

* All features
  trend all features overall energy - 0.12122230231761932 (Pictures)
  forecast all features overall energy - 0.545603564188161 (Pictures)
  
* Best
  forecast test MAE - 0.41751142945194003 (PICTURE)

<br /><br />

## Limitations and Suggestions

<br /><br />

## Sources

<br /><br />
