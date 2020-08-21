# London Energy Forecast

Using an LSTM model to forecast energy consumption in London (analysis by Acorns included).

*Authors*: Omar Alejandro Martínez and Nicolás Rosal
<br /><br />


## Datasets

The datasets required can be found in the following [Kaggle link](https://www.kaggle.com/jeanmidev/smart-meters-in-london)

<br />


## Purpose

The purpose of this project was to build a deep learning model capable of recognizing trends in energy consumption and generating forecasts of future usage levels. All this to help:

* Energy Companies understand user consumption trends and better generate consumption estimates in the future.
* Users by providing information about their future energy consumption levels (one month ahead). 
* The local government better understand the overall energy consumption patterns and future behaviour in London. 
* Any interested party by providing a framework that allows them to further explore how acorns, contract type, seasons, etc. (other features) can be useful at generating more accurate forecasting models. 

<br />

 
## Useful Links 

Acorn User Guide to understand consumer classification: [guide](https://acorn.caci.co.uk/downloads/Acorn-User-guide.pdf)

<br />

## Methodology

### 1. Cleaning and Feature Engineering ([jupyter notebook](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Data%20Cleaning%20-%20Feature%20Engineering.ipynb))([nbviewer version](https://nbviewer.jupyter.org/github/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Data%20Cleaning%20-%20Feature%20Engineering.ipynb))

The goal of this section was to clean and combine all available datasets into a single dataset that would be easier to handle for exploratory data analysis. Moreover, during the process, the following features were engineered to further explore energy consumption:

* Segmentation of Average Energy Consumption by Parts of Day -- It was hypothesized that average energy consumption could be divided into different parts of the day, namely morning, afternoon, evening, etc. To test this hypothesis, halfhourly average consumptions vs. time was plotted. As a result, four regions where consumption trends shifted were identified. These regions were used to segment the data into Dawn, Morning-Afternoon, Evening and Night parts of day according to [Merriam-Webster's division](https://www.learnersdictionary.com/qa/parts-of-the-day-early-morning-late-morning-etc).<br />
![Average Consumption by Part of Day](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/averagepartsofday.png)
<br />

* Holiday Binary Feature -- The information contained in the holiday dataset was used to create a binary column that identified which days were holidays (1) and which were not (0). 
<br />

* Season Clustering -- It was hypothesized that energy consumption would vary depending on seasons. Hence, the weather data was clustered using FAMD (Factor Analysis for Mixed Data) components and a K-Means algorithm. Originally, it was planned to use PCA analysis but, given the data presence of different data types (numerical and categorical features), a FAMD analysis was required. The final clusters were not defined by strict season divisions given that end of spring - start of autumn and start of spring - end of autumn would present similar weather conditions. For this reason, the data was grouped by 'Winter', 'Summer', 'StartSpr/EndAut', and 'EndSpr/StartAut'. <br />

NOTE: After the 'Clustering for Feature Engineering Section', a 3D plot of the cluster could not be visualised in the jupyter notebooks uploaded. This is that plot: <br />
![3D Clustering Plot](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/cluster.png)
<br />


### 2. Exploratory Data Analysis ([jupyter notebook](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Exploratory%20Data%20Analysis.ipynb))([nbviewer version](https://nbviewer.jupyter.org/github/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Exploratory%20Data%20Analysis.ipynb))

The goal of this section was to further explore the data set created after 'Cleaning and Feature Engineering' and prepare the data set for the 'Modelling' section. 

* Plotting Distributions -- After plotting the distributions of each variable, it was identified that energy-related features (e.g. energy_sum, Avg.Dawn, etc.) and visibility presented a highly skewed distribution. This gave evidence of the possible presence of outliers in the data.
<br />

* Outliers Analysis -- It was verified that 'visibility', 'energy_sum', 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening', and 'Avg.Night' presented outliers. Thus, these would be explored to understand whether the outliers would be reasonable given the scale and nature of the data. In this case, it was evidenced that most of the visibility outliers occur in 'Winter' and 'StartSpr/EndAut' which makes makes sense given that visibility decreases during colder periods. Similarly, it has been found that households tend to consume more energy during colder climates. Hence, 'visibility', 'energy_sum', 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening', and 'Avg.Night' outliers will be kept. <br />
<img src="https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/visibility.png" width="450"/> <img src="https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energy_sumout.png" width="450"/>

* Correlation Analysis -- As a result of the analysis, several variables such as 'temperatureLow' and 'temperatureHigh' were eliminated to reduce high dimensionality and prevent high correlation problems. Similarly, a very strong correlation between 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening','Avg.Night', and 'energy_sum' was identified. Nevertheless, all these features were kept for modelling but carefully considered in order to avoid undesired outcomes due to strong correlations. All other features presented a low correlation with 'energy_sum', leading to the assumptions that they will have minimum impact on the models. For this reason, a feature importance analysis was performed later on. <br />
![Heatmap](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/heatmap.png)

* Energy Sum Analysis Over Time -- Plotting energy consumption over time helped to understand how consumption varies throughout the years (months/seasons). Further analysis was made with the different energy sum averages per part of day. It was found that energy consumption in London has a clear seasonality, with higher consumption during months closer to winter and lower consumption during months closer to summer. <br /> 
![Energy Sum over Time](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energyvstime.png)

* Energy Sum per Type of Contract -- Plotting energy consumptions per contract type allows to see differences in behaviour between different type of clients. Although there is a difference in usage between contract types, it was hypothezised that 'time of use' contracts would exhibit significantly lower consumption levels than 'standard' contracts. Nevertheless, the findings showed that the difference was not as significant as expected. <br /> 
![Energy Sum vs. Contract Type](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/ContractType.png)

* Energy Sum per Acorn -- Plotting energy consumption per Acorn helped analyse whether belonging to different acorn groups would affect consumption patters and behaviours. It was found that Acorn A, Acorn D, and Acorn U were the top three acorns in terms of overall energy consumption. Moreover, this analysis showed that, on average, households in all Acorns tend to consume more energy during the Evening. <br /> 
![Energy Sum vs. Acorn](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/acorns.png)

* Feature Importance -- To reduce the complexity of the data, a Random Forest's feature importance attribute was used to discover the features that would be important to add in the final dataset. Due to poor visualisations, a picture of the feature importance bar plot created is included below. In here, 'Avg.Dawn', 'Avg.Morning_Afternoon', 'Avg.Evening', and 'Avg.Night' were identified as the most important features. This finding, together with those of the heatmap correlation analysis, led to the decision of selecting only these four features for the modelling section. <br /> 
![Feature Importance](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/featureimportance.png)

<br /><br />

### 3. Modelling ([jupyter notebook](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Modeling.ipynb))([nbviewer version](https://nbviewer.jupyter.org/github/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Modeling.ipynb))

The goal of this section was to build an LSTM model capable of fitting energy trends and forecasting energy consumption.

* Why LSTM? -- Due to the nature of the problem, a model that was capable of dealing with time-series data was necessary. In this case, LSTM was chosen over GRU due to its capacity to remember longer sequences that would be needed to compute more accurate forecasts. <br /> 

* Why different architectures? -- It was found that a TimeDistributed output layer reduced significantly the MAE of forecasting tasks. Thus, this model was created with two different output layers that could change to fit the assigment to be performed (Trend recognition or Forecasting).<br /> 

* Tuning (Scores) -- The model was tuned by changing the number of LSTM layers and the number of LSTM units per layer, using dropout, l1, l2, and batch normalization regularizations, and modifying the optimizer and output layer. The best performing architecture for forecasting consisted of five LSTM layers with 64 Units, Dropout of 25%, one dense time distributed output layer, compiled with a RMSProp optimizer.<br /> 


#### Outcomes
<br /> 

* Overall Energy in London Model 

| Trend Test MAE | 
| -------------- | 
| Score: 0.0597  |  
|    ![Trend](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energytrend.png)     |
<br />

| Forecast Test MAE |  
| ----------------- | 
| Score: 0.41751    |  
|     ![Forecast](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energyforecast.png)      | 

<br /> <br /> 

* Energy Model by Acorn

| Trend Test MAE | 
| -------------- | 
| ![Trend](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/acorntrend.png)  |  
<br />

| Forecast Test MAE |  
| ----------------- | 
|      0.68622      | 
| ![Forecast](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/acornforecast.png)    |  

 <br /> <br /> 
  
* Combined Models 

| Forecast Test MAE Overall Energy | 
| -------------------------------- | 
| Score: 0.43439  |  
|    ![Overall Forecast](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/energyforecastcombined.png)     |
<br />

| Forecast Test MAE Acorn | 
| ----------------------- | 
| Score: 0.63155 |  
|    ![Acorn Forecast](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/acornforecastcombined.png)     |

 <br /> <br />
 
* All Features Model

| Trend All Features Overall Energy Test MAE | 
| ------------------------------------------ | 
| Score: 0.12122  |  
|    ![All Features Forecast Overall Energy](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/trendallfeatures.png)     |
<br />

| Forecast All Features Overall Energy Test MAE | 
| --------------------------------------------- | 
| Score: 0.54560 |  
|    ![Acorn Forecast](https://github.com/nicolasrosal98/London-Energy-Forecast/blob/master/Images/forecastfeatures.png)     |
<br />
  
  
## Conclusions, Limitations and Suggestions

* Conclusions

1. The findings of this project suggest that by looking at previous energy consumption levels (one month worth of readings) it is possible to forecast one month into the future with just 0.417 kWh overall error (best model).  
2. The model built using the overall energy consumption data of London presented the lowest trend and forecast MAE out of all the models. Even though some acorn models showed lower trend MAE, the overall acorn model's trend mean squared error was not lower than that of the Overall Energy in London Model. 
3. It is interesting that using the model trained with the overall energy consumption data to forecast energy use per acorn returned better results than the model trained with each acorn independently. 
4. The team considered adding other engineered features to the model but these did not help the model return better results.
<br />

* Limitations

1. Computational Power -- Given constraints in this area, more extensive and throurough model tuning could not be performed. For example, the team intended to build a model trained by individual consumers (LCLid) but, given the higher dimensionality of the data (5560 LCLids with over 600 entries per ID), training this model was not feasible. 
<br />

* Suggestions for further research

1. Feature Selection -- It would interesting to include other variables, such as 'Season', 'Holiday', and 'StdorToU', into the model even though these were not identified as important features by the random forest. 
2. Acorn Information -- Even though the team was able to include Acorns into the analysis, it would be interesting to explore how to include acorns' demographic and socioeconomic details and whether these contribute to better model performance. This data can be found here: [acorn_details.csv](https://www.kaggle.com/jeanmidev/smart-meters-in-london?select=acorn_details.csv) 
3. Forecasting Energy Consumption by User -- Using energy consumption data per LCLid to train a model. 
4. Combine Daily Data into Month -- Combine daily data per LCLid into months to train a model capable of forecasting several months into the future. 

<br /><br />

## Sources

Humam Fauzi. "Multiple Stock Prediction using Single NN. Kaggle". [link](https://nbviewer.jupyter.org/github/nicolasrosal98/London-Energy-Forecast/blob/master/Jupyter%20Notebooks/Exploratory%20Data%20Analysis.ipynb).

Ian Felton. "A Quick Example of Time-Series Prediction using Long Short-Term Memory (LSTM) Networks". Medium. [link](https://medium.com/swlh/a-quick-example-of-time-series-forecasting-using-long-short-term-memory-lstm-networks-ddc10dc1467d)

Jason Brownlee. "How to Develop LSTM Models for Time Series Forecasting". Machine Learning Mastery.[link](https://machinelearningmastery.com/how-to-develop-lstm-models-for-time-series-forecasting/)

jrhea. "Energy Consumption Forecast". Kaggle Notebook. [link](https://www.kaggle.com/rheajgurung/energy-consumption-forecast)

Max Halford. "FAMD". Prince Documentations. [link](https://github.com/MaxHalford/prince#factor-analysis-of-mixed-data-famd)
<br /><br />

## About the Authors

* LinkedIn: 

[Omar Alejandro Martínez](https://www.linkedin.com/in/omar-martinez-garcia-9b2214176/)

[Nicolás Rosal](www.linkedin.com/in/nicolasrosal98)
