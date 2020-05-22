# Covid19-Outbreak-in-New-York-City

NYC Health's novel coronavirus contraction by ZCTA's (Zip Code Tabulation Area) dataset is examined in this repository. The number of positive cases and percentage of positive cases by Zip Code will be modelled using several demographic, economic, and social indicators. Please check the .ipynb files for the codes used in this project. Link for accessing the data:

NYC Health's coronavirus data (updated daily)

https://github.com/nychealth/coronavirus-data/blob/master/tests-by-zcta.csv

American Community Survey 2014-2018 Profile Report via the Missouri Census Data Center (original source: U.S Census Bureau)

https://census.missouri.edu/acs/profiles/ 

The combined dataset consists of the following indicators for 177 Zip Codes that constitute New York City (NYC). Each of them is either an Economic, Demographic, or Social indicators.

1. __POSITIVES__: The number of positive Covid19 cases
2. __PERCENT_POSI__: The percentage of positive Covid19 cases 
3. __DEGREES__: The percentage of the population of 25 years or older to possess a Bachelor degere or higher (Social)
4. __ENROLLS__: The percentage of the population of 3 years nad over to be in college or graduate school (Social)
5. __ELDER__: The percentage of the total population at age 65 years or older (Demographic) [1]
6. __WHITES__: The percentage of the total population who is white (Demographic) 
7. __BLACKS__: The percentage of the total population who is black or African American (Demographic) [2]
8. __ASIANS__: The percentage of the total population who is Asians (Demographic) [2]
9. __MINORITIES__: Sum of __BLACKS__ and __ASIANS__ (Demographic)
10. __MED_INCOMES__: Median household income (Econoimic) [3]
11. __MEAN_INCOMES__: Mean household income (Economic) [3]
12. __POVERTY__: The percentage of the total population that is living below the poverty line (Economic) [4]
13. __UNEMPLOYMENT__: Unemployment Rate (Economic) 

[1] https://www.cdc.gov/coronavirus/2019-ncov/need-extra-precautions/older-adults.html  

[2] https://www.cdc.gov/coronavirus/2019-ncov/need-extra-precautions/racial-ethnic-minorities.html 

[3] https://www.nytimes.com/2020/03/01/upshot/coronavirus-sick-days-service-workers.html

[4] https://www.theatlantic.com/ideas/archive/2020/03/coronavirus-will-supercharge-american-inequality/608419/


## Dataset 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/main_dataframe.JPG">
</p>

#### The data for Covid19 Positive Tests and Percentage of Positive Tests are updated as of May 11th, 2020. 


## Zip Code Coverage Visualization

The following shows the areas of NYC that the data will represent

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/zipcode_coverage.JPG">
</p>

## Choropleth Maps 

### Percent of Positive COVID19 Tests 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/percent_map.JPG">
</p>


### Population that is 65 years or older 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/elder_map.JPG">
</p>

### Population of Minorities (majorly African American and Asian)

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/minorities_map.JPG">
</p>

### Median Household Income 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/medianincome_map.JPG">



Let's start exploring the dataset. For the purpose of this project, only the Percentage of Positive Covid19 test ("__PERCENT_POSI__") will serve as the target feature.

## Heatmap of the correlations between the different features:

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/corr_heatmap.JPG">
</p>

* The following highly correlated features will be examined for Multicollinearity:
1. __MEAN_INCOMES__ and __MED_INCOMES__ (Economic Indicators)
2. __MINORITIES__, __WHITES__, and __BLACKS__ (Demographic Indicators)


## Skewness of target (__PERCENT_POSI__)

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/skew_map.JPG">
</p>

* The data shows a skewness of ~ -0.592 which is very close to being approxmiately symmetric. Can forgo transformation. 


## p-value Evaluation 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/ini_pvalue.JPG">
</p>

Observations:
* __ENROLLS__, __ELDER__, __ASIANS__, __MED_INCOMES__, and __UNEMPLOYMENT__ have very high p-values.
* This will be taken into consideration during model reduction/modification 
* The Economic, Demographic, and Social data are 5-year estimates from 2014-2018. This is due to the fact that NYC only takes it census every 10 years. Since the last census was taken in 2010, using data from then would be too inaccurate to represent NYC in 2020. 
* Covid19 data from NYC Health is also incomplete which may have contributed to the high p-values. 

## Multicollinearity (Variance Inflation Factor)

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/VIF.JPG">
</p>

* VIF for all coefficients are < 10  except for BLACKS, ASIANS, MINORITIES (Group 1) and MED_INCOMES and MEAN_INCOMES (Group 2). This suggests multicollinearity exists within these two groups, especially in Group 1. 
* Will need to drop either BLACKS + ASIANS or MINORITIES in Group 1, and either MED_INCOMES or MEAN_INCOMES in Group 2 

## Baysian Information Criterion

|                                                   | BIC |
|---------------------------------------------------|-----|
| Original Model                                    | 797 |
| Reduction #1: MED_INCOMES                         | 792 |
| Reduction #2: MEAN_INCOMES                        | 795 |
| Modified #1: no MEAN_INCOMES, BLACKS, and ASIANS  | 791 |
| Modified #2: no MEAN_INCOMES, BLACKS, and ASIANS  | 788 |
| Modified #3: no MEAN_INCOMES and MINORITIES       | 795 |


* Modified #2 has the lowest BIC, therefore it is preferred.

The p-values for the features of these models are recalculated and shown below.

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/new_pvalue.JPG">
</p>

* "Modified model 2" seems to have the best combination of p-values as well

The VIFs for this model is also recalculated and shown below

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/VIF_new.JPG">

* All features have VIFS below 10, which is good. Can go ahead and choose this model for Linear Regression. 

## Linear Regression 

### Multiple Linear Regression 

#### Reduced Model

Train Dataset 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/reduced_train.JPG">


Test Dataset 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/reduced_test.JPG">


#### Original Model 
 
 Train Dataset 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/full_train.JPG">

Test Dataset 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/full_test.JPG">

#### Linear Regression Metrics 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/metric_mlr.JPG">

Observations 
* The Reduced Model has approximately the same Correlation coefficients, R-Squared, and MSE as the Original model for both train and test data 
* The former, however, benefits from having features with much more improved p-values and less multicollinearity 

### Polynomial Regression 

#### Deg = 2 

Train Data 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/poly2_train.JPG">

Test Data 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/poly2_test.JPG">


#### Deg = 3 

Train Data 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/poly3_train.JPG">

Test Data 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/poly3_test.JPG">

#### Combined Metrics 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/metric.JPG">

* Both Polynomial Regression models resulted in overfitting
* Polynomial deg = 3 model was very inaccurate in predicting using the test dataset 

### Ridge Regression 

To improve the Polynomial model, Ridge Regression is used. The optimal alpha was found to be 1 (code and result found in "Covid19.ipynb" file)

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/ridge.JPG">

Correlation coefficient: 0.81
R-Squared: 				 0.53
MSE: 					 15.0 

* Ridge Regression did improve the Polynomial model. However, it is still not as accurate and reliable as the Reduced and Full Models. 


## The Reduced Model is the best model

The coefficients for the model's features are shown below 

<p align="center">
  <img src="https://github.com/sonleh96/NYC-Covid19/blob/master/Charts%20and%20Graphs/model_coef.JPG">


## Future Improvements 

* Update datasets 
* Find more accurate and reliable data for Economic, Demographic, and Social indicators 
* More analysis on the zip codes that are the most affected by the pandemic 
* Show statistical differences between the five boroughs 