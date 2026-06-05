# **Exploratory Data Analysis Report**

Gridlock Hackathon 2.0 — Traffic Demand Prediction

Team: HackHer11

Date:  5th June 2026



This report documents the Exploratory Data Analysis conducted on the Gridlock Hackathon 2.0 dataset. The primary objective of this competition is to predict traffic demand values (ranging from 0 to 1) for 41,778 test data points across various locations and time slots.

#### &#x20;**MISSING VALUR ANALYSIS** 

A missing value analysis was performed across 

all 11 columns. The following results were found:



Column          Missing Count    Missing Percentage

────────────────────────────────────────────────────

Index                0               0.00%

geohash              0               0.00%

day                  0               0.00%

timestamp            0               0.00%

demand               0               0.00%

NumberofLanes        0               0.00%

LargeVehicles        0               0.00%

Landmarks            0               0.00%

RoadType           600               0.78%

Weather            797               1.03%

Temperature       2495               3.23%



Key Findings:

Three columns contain missing values. Temperature has the highest number of missing values at 2,495 records which accounts for 3.23% of the total training data. Weather follows with 797 missing records (1.03%) and RoadType has 600 missing records (0.78%).



The most critical columns for modelling such as geohash, day, timestamp and the target variable demand have zero missing values, which confirms the integrity of the core dataset.

##### **recommendation:**

**Since RoadType and Weather are categorical variables, missing values were replaced with the most frequently occurring value. For Temperature, the median was used because it is not heavily influenced by unusually high or low readings.**



### **Statistical Summary**

**The target variable "demand" represents traffic** 

**demand at a specific location and time. It is a** 

**continuous variable ranging from 0 to 1.**



**The following statistics were observed:**



**Total Records : 77,299**

**Mean          : 0.0939**

**Median        : 0.0478**

**Std Deviation : 0.1422**

**Minimum       : 0.0000**

**Maximum       : 1.0000**

**25th Percentile (Q1) : 0.0182**

**75th Percentile (Q3) : 0.1086**



**The large difference between the mean (0.0939) and the median (0.0478) strongly indicates that the distribution is right skewed. This means the majority of traffic locations experience very low demand while a small number of locations experience extremely high demand.**



### distribution analysis 

**Visual Patterns: The histogram and boxplot show that traffic demand is heavily concentrated near zero, with a long right tail extending all the way to 1.0.**

**Severe Right Skew: A calculated skewness value of 3.73 confirms a highly positive skew in the dataset.**

**Traffic Insights: This reflects typical real-world conditions—most locations experience low demand, while only a few critical areas (like highways and major junctions) see high demand.**

**Modeling Action: Standard regression models struggle with this level of skewness; apply a log transformation (like $\\log(x + 1)$ to handle the zeros) before training to improve performance.**



### outlier analysis 

**Outliers were detected using the IQR method which identifies values that fall below Q1 - 1.5\*IQR or above Q3 + 1.5\*IQR.**



**Q1 (25th Percentile) : 0.0182**

**Q3 (75th Percentile) : 0.1086**

**IQR                  : 0.0904**

**Lower Bound          : -0.1173**

**Upper Bound          : 0.2441**



**Total Outliers Found : 6,413 rows**

**Outlier Percentage   : 8.30%**



**6,413 records (8.30% of training data) were identified as outliers with demand values exceeding 0.2441. These are not errors in the data. They represent real locations with genuinely high traffic demand such as highways, major intersections, and busy urban areas.**





### **Feature Analysis and Visualizations**

###### **1.demand vs hour of day :**

**Traffic demand peaks at midday (11 AM–1 PM) and hits its lowest point at 7 PM, showing that activity is driven by midday travel rather than traditional rush hours.Because of this clear pattern, the hour of day is a critical feature for the predictive models.**



###### **2.demand vs road type :** 

**The bar chart clearly shows that Highway roads experience dramatically higher traffic demand compared to other road types. Highway demand (0.61) is more than twice that of Street demand (0.27) and more than ten times that of Residential demand (0.057).**



###### **3.demand vs lanes:**

**Roads with 1 to 3 lanes have very similar and low average demand values around 0.08. However roads with 4 or 5 lanes show dramatically higher demand values around 0.60.**



**This sharp jump at 4 lanes strongly suggests that roads with 4 or 5 lanes correspond to highways and major arterial roads which naturally carry much higher traffic volumes.**

###### 

###### **4. demand vs large vehicles:**

**Locations where large vehicles are allowed show significantly higher average demand** 

**(0.130) compared to locations where they are not allowed (0.074). This is almost double the demand.**



###### 

###### **5.demand vs geohash:**

**The chart shows significant variation in demand across different geographic locations. The top location qp09d9 has an average demand of 0.96 which is very close to the maximum possible value of 1.0. This indicates an extremely busy traffic location.**



**The top 3 locations all start with the prefix "qp09" suggesting they are geographically close to each other and form a high traffic cluster or hotspot in the city.**



###### **6. correlation heatmap:**

**NumberofLanes has the strongest correlation with demand at 0.21. This confirms that roads with more lanes tend to have higher traffic demand.**



**Hour has a very weak negative correlation of -0.03 with demand. This does not mean hour is unimportant. The weak linear correlation is because the relationship between hour and demand is non-linear (demand peaks at midday and drops in evening) which a simple correlation coefficient cannot capture.**



**Temperature shows essentially zero correlation with demand suggesting temperature alone is not a strong linear predictor.**





###### **day analysis :**

**The training data contains records for days 48 and 49 only. The test data contains records for day 49 only.**



**Train day 49 timestamp range : 00:00 to 02:00**

**Test day 49 timestamp range  : 02:15 to 13:45**



**Although both train and test share day 49, the timestamp ranges do not overlap. This means a direct lookup approach is not** 

**possible and a proper machine learning model must be trained to predict demand for the test timestamps.**



##### **Prediction Error Analysis**

**To better understand where the XGBoost model struggles, the predictions were examined by grouping demand into high and low traffic levels. This helped identify cases where the model predicted high demand when the actual demand was low, and vice versa.**



**False Positives (FP): The model predicted high traffic demand, but the actual demand was low.**

**False Negatives (FN): The model predicted low traffic demand, but the actual demand was high.**



**Looking at these cases gives a better idea of where the model tends to overestimate or underestimate traffic demand. Since traffic conditions are affected by many factors that may not be fully captured in the dataset, some prediction errors are expected. This analysis helps explain why achieving perfect accuracy is difficult in real-world traffic forecasting.**



### **top 10 observations**



**1. The training dataset contains 77,299 rows with 11 columns and has zero duplicate records, confirming high data quality.**



**2. Three columns have missing values: RoadType (600 records, 0.78%), Weather (797 records, 1.03%) and Temperature (2,495 records, 3.23%). All other columns are complete.**



**3. The target variable demand is highly right skewed with a skewness of 3.73, meaning most locations have very low traffic demand while a few locations experience very high demand.**



**4. The median demand is only 0.0478 while the mean is 0.0939, confirming that the distribution is heavily skewed to the right.**



**5. A total of 6,413 rows (8.30% of training data) are statistical outliers with demand values above 0.2441. These represent real high traffic scenarios and should be retained for training.**



**6. Highway roads have an average demand of 0.61 which is more than 10 times higher than Residential roads (0.057), making roadType one of the strongest predictors of demand.**



**7. Traffic demand peaks at 11:00 AM with an average demand of 0.118 and is lowest at 7:00 PM with an average demand of 0.042, showing a clear midday traffic pattern.**



**8. Roads with 4 or 5 lanes have dramatically higher demand (0.60) compared to roads with 1 to 3 lanes (0.08), indicating that lane count is strongly linked to road type and traffic volume.**



**9. The top traffic location qp09d9 has an average demand of 0.96, nearly at maximum  capacity, while location clusters starting with prefix qp09 consistently show the  highest demand values.**



**10. Locations where large vehicles are allowed show 75% higher demand (0.130) compared to locations where they are not allowed (0.074), confirming that large vehicle access is a useful predictor of traffic demand.**

