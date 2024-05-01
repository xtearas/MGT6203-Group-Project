# Cool, Quiet City Competition - Predicting thermal preference with Digital Twin Metrics 

*By Silvia Vangelova, Jack Li, Joanna Stefaniak, Chengxuan Feng, Max Midlash; Georgia Institute of Technology*

## Data Preparation
The data used in this analysis comes from a Kaggle competition using smartwatch data from users in Singapore \[1\]. We have two types of data sets: user response logs combined with their smartwatch readings, and meteorological measurements from specific weather stations in Singapore about humidity, wind, temperature, and rainfall. One important characteristic that both types of datasets have in common is that they are both time-series datasets. Their date-time columns have been converted to time objects. In addition, we have a dataframe with the IDs and locations of all weather stations in the city from which the measurements were taken.

### User Response Data

The user response dataset consists of two parts - one for training, with 1,149,136 rows, and one for testing, with 996,429 rows. The test to training ratio is 0.88, meaning 46% of the total user response data can be used to compare the quality of different types of models. To do so we further divide the training dataset into two parts - 80% of the data is used for model fitting and 20% is used for model cross-validation. After we have chosen the best of each type of model we can then compare them based on their performance on the remaining test dataset.

![img](./images/thermal_preference_map.png?raw=true)

The user response dataset consists of two parts - one for training, with 1,149,136 rows, and one for testing, with 996,429 rows. The test to training ratio is 0.88, meaning 46% of the total user response data can be used to compare the quality of different types of models. To do so we further divide the training dataset into two parts - 80% of the data is used for model fitting and 20% is used for model cross-validation. After we have chosen the best of each type of model we can then compare them based on their performance on the remaining test dataset.

Despite having many observations, most of the data are NaN or empty strings. This is because the dataset is composed of three types of data - physiological measurements (smart watch readings), spatial characteristics, and micro-survey logs. The physiological measurements are frequent at regular intervals. When the user logs a survey response, the physiological measurements stop. Longitude and latitude data are then stored and used to calculate spatial characteristics at that location using the Urbanity framework \[2\]. So, a row from the user survey response dataset would have either physiological measurements or spatial characteristics and microsurvey logs. 

We are interested in the thermal preference response. We hypothesize that some variables that influence a person's perception of temperature are current heart rate and current activity level, such as total steps taken or total distance walked 10 minutes prior to recording the thermal preference response. For each user, we take smartwatch readings 10 minutes prior to each user's survey response. From these data we take average heart rate and total distance walked and add them as variables to our models.
In the training dataset, out of all 996,429 rows, only 4,900 contain survey response logs. 1,943 users wish it were cooler, 304 wish it were warmer and 2,652 wish no change.  Most users were located indoors: Indoor - Class: 294 logs, Indoor - Home: 2,200 logs, Indoor - Office: 872 logs, Indoor - Other: 660 logs, Outdoor: 568 logs, Transportation: 306 logs.

### Meteorological Data

We also have meteorological data about temperature, rainfall, humidity and wind at various different weather stations. Measurements in all datasets start from October 9th 2022 and end July 3rd 2023. Temperature, humidity and wind are measured 1,347 times a day on average, or in other words, about every minute.  Rainfall is measured 276 times a day on average, or in other words, about every five minutes. Naturally, the timestamps of weather station measurements differ from the time stamps of user survey response logs. All weather stations have days in which no weather data is reported. Each user’s data reported the latitude and longitude at the time of response. We were able to use this latitude and longitude data to find the weather station located closest to each user. Then, by leveraging the time of response combined with the nearest weather station, we were able to determine the various meteorological data most relevant to the user. 


![img](./Visualizations/tree_model.png?raw=true)

In order to verify the results, the Project Final Report Notebook has to be run in R studio from top to bottom. Tere is also an html version of the Notebook for faster review. The notebooks can be found in the Project Final Report directory.

required libraries are:

- library(lubridate)
- library(ggplot2)
- library(dplyr)
- library(plotly)
- library(visdat)
- library(tidyr)
- library(data.table)
- library(raster)
- library(nnet)
- library(purrr)
- library(DataExplorer)
- library(pscl)
- library(tree)
- library(rpart)
- library(rpart.plot)
- library(ISLR)
- library(randomForest)
- library(kableExtra)
- library(broom)
- library(rattle)	
- library(corrplot)


Required data sets are:

cozie_responses_and_physiological_data_training.csv
- weather_rainfall.csv
- weather_wind-speed.csv
- weather_wind-direction.csv
- weather_air-temperature.csv
- weather_relative-humidity.csv
- weather_stations.csv

They can be obtained from: https://www.kaggle.com/competitions/cool-quiet-city-competition/overview

Make sure they are in the same directory as the Notebook

Reference:

\[1\] Clayton Miller, Matias Quintana, Mario Frei, Yun Xuan Chua, Chun Fu, Bianca Picchetti, Winston Yap, Adrian Chong, and Filip Biljecki. 2023. Introducing the Cool, Quiet City Competition: Predicting Smartwatch-Reported Heat and Noise with Digital Twin Metrics. In Proceedings of the 10th ACM International Conference on Systems for Energy-Efficient Buildings, Cities, and Transportation (BuildSys '23). Association for Computing Machinery, New York, NY, USA, 298–299. https://doi.org/10.1145/3600100.3626269

\[2\]  Yap, W., Biljecki, F. A Global Feature-Rich Network Dataset of Cities and Dashboard for Comprehensive Urban Analyses. Sci Data 10, 667 (2023). https://doi.org/10.1038/s41597-023-02578-1 

