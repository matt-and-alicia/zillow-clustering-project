# <a name="top"></a>Zillow: Predicting the Error 
## README.md
![](http://zillow.mediaroom.com/image/Zillow_Wordmark_Blue_RGB.jpg)

by: Alicia Gonzalez and Matthew Dalton

<p>
  <a href="https://github.com/mdalton87" target="_blank">
    <img alt="Matthew" src="https://img.shields.io/github/followers/mdalton87?label=Follow_Matt&style=social" />
  </a>
  <a href="https://github.com/orgs/matt-and-alicia/people/aliciag92" target="_blank">
    <img alt="Alicia" src="https://img.shields.io/github/followers/aliciag92?label=Follow_Alicia&style=social" />
  </a>
  <a href="https://github.com/matt-and-alicia/zillow-clustering-project" target="_blank">
    <img alt="Forks" src="https://img.shields.io/github/forks/matt-and-alicia/zillow-clustering-project?style=social" />
  </a>
  <a href="https://github.com/matt-and-alicia/zillow-clustering-project/commits/main" target="_blank">
    <img alt="Last: Push" src="https://img.shields.io/github/last-commit/matt-and-alicia/zillow-clustering-project" />
  </a>
</p>


***
[[Project Description](#project_description)]
[[Project Planning](#planning)]
[[Key Findings](#findings)]
[[Data Dictionary](#dictionary)]
[[Data Acquire and Prep](#wrangle)]
[[Data Exploration](#explore)]
[[Statistical Analysis](#stats)]
[[Modeling](#model)]
[[Conclusion](#conclusion)]
___


## <a name="project_description"></a>Project Description:
[[Back to top](#top)]

Zillow is one of the most popular real estate databases online. One of Zillow’s key features is its Zestimates, a popular consumer tool for seeing how much homes are worth.

Zestimates offer users a starting point in home valuation, but these numbers may not be as accurate as one might think for a variety of reasons and there may still be some error.

That is why Zillow's dataset includes the log error information, which is the difference between sales price and estimated price.

## Goals

- Identify the drivers for errors in Zestimates by incorporating clustering methodologies.
- Document the process and analysis throughout the data science pipeline.
- Demonstrate our process and articulate the information that was discovered.
- Deliverables:
    - README.md file containing overall project information, how to reproduce work, and notes from project planning.
    - Jupyter Notebook Report detailing the pipeline process.
    - Python modules that automate the data acquistion, preparation, and exploration process. 

***
## <a name="planning"></a>Project Planning: 
[[Back to top](#top)]

### Project Outline:

For this project we are working with the Zillow dataset using the 2017 properties and predictions data for single unit / single family homes.

This notebook consists of discoveries made and work that was done related to uncovering what the drivers of the error in the zestimate is.

- Acquisiton of data through Codeup SQL Server, using env.py file with username, password, and host
- Prepare and clean data with python - Jupyter Labs / Jupyter Notebook
- Explore data
    - if value are what the dictionary says they are
    - null values
        - are the fixable or should they just be deleted
    - categorical or continuous values
    - Make graphs that show relationships
- Use clustering to create a subsection of the data
- Run statistical analysis
- Model data using regression algorithms
- Test Data
- Conclude results
        
### Hypothesis

- Is there a correlation between square footage of a home and log error?
- Is there a relationship between tax value and log error?
- Could there be clusters in age, acres, or location that can help predict log error? 

### Target variable

- We are targeting the **log_error** in this project

### Need to haves (Deliverables):

- Final Notebook
    - Presentation from notebook
- README that explains what the project is, how to reproduce you work, and your notes from project planning.
- Python module or modules that automate the data acquisistion and preparation process


- Data Acquisition: 
    - Data is collected from the codeup cloud database with an appropriate SQL query
- Data Prep: 
    - Column data types are appropriate for the data they contain
    - Missing values are investigated and handled
    - Outliers are investigated and handled
- Exploration:
    - interaction between independent variables and the target variable is explored using visualization and statistical testing
    - clustering is used to explore the data. 
    - A conclusion, supported by statistical testing and visualization
    - At least 3 combinations of features for clustering should be tried
- Modeling: 
    - At least 4 different models are created and their performance is compared. One model is the distinct combination of algorithm, hyperparameters, and features.
- Best practices on data splitting are followed
- The final notebook has a good title and the documentation within is sufficiently explanatory and of high quality
- Decisions and judment calls are made and explained/documented
- All python code is of high quality


### Nice to haves (With more time):

- Would like to explore 3-D clustering by adding latitude or acres
- More time to explore less common features like a/c unit type and fireplaces
- Would like to get more time to fill out the missing data so that there are even more data points to work with


***

## <a name="findings"></a>Key Findings:
[[Back to top](#top)]

### Explore:
- log_error doesn't correlate very well with anything
    - correlation with prop_sqft is the highest
- log_error is highest in Orange county
- T-test resulted in rejecting the null hypothesis meaning we can assume that there is a difference in log_error between counties
- Results show log_error is significantly different for properties in LA County vs Orange County vs Ventura County
- Kmeans:
    - acres and age
    - latitude and longitude
    - prop_sqft and tax_value


### Stats
We ran 1 - ANOVA Test and 4 - One-Sample, Two-Tailed, T-test

Both Statistics tests were run at a 95% confidence interval

- ANOVA results:
    - f = 10.3375
    - p-value = 8.5052 x 10-7
    - We reject the null hypothesis that log_error is the same across all 4 clusters
    - We move forward with the alternative hypothesis that The log_error is different in the clusters

- T-test: 
    - Clusters 0, 1, and 2 reject the null hypothesis and are good candidates for modeling
    - Cluster 3 failed to reject the null hypothesis and are not suitable for modeling.

| Cluster | t-statistic | p-value | Reject Null? |
| ------- | :-------: | :-------: | :-------: |
| Cluster0 | -2.4378 | 0.0148 | Yes |
| Cluster1 | -4.4752 | 7.7006 x 10<sup>-6</sup> | Yes |
| Cluster2 | 2.7658 | 0.0057 | Yes |
| Cluster3 | 1.2522 | 0.2106 | No |

    
### Modeling:
| Model | Validation/Out of Sample RMSE | R<sup>2</sup> Value |
| ---- | :----:| ----: |
| Baseline | 0.167366 | 2.2204 x 10<sup>-16</sup> |
| Linear Regression (OLS) | 0.166731 | 2.1433 x 10<sup>-3</sup> |  
| Tweedie Regressor (GLM) | 0.155186 | 9.4673 x 10<sup>-4</sup>|  
| Lasso Lars | 0.166731 | 2.2204 x 10<sup>-16</sup> |  
| Quadratic Regression | 0.027786 | 2.4659 x 10<sup>-3</sup> |  

- Quadratic Regression model performed the best
     - The RMSE of the validation/out of sample had the largest reduction from the baseline.
     - Also, this model has the highest R<sup>2</sup> value
- Quadratic Testing:
    - RMSE = 0.160752	
    - R<sup>2</sup> value = -3.9361  x 10<sup>-4</sup>

***

## <a name="dictionary"></a>Data Dictionary  
[[Back to top](#top)]

### Data Used
---
| Attribute | Definition | Data Type |
| ----- | ----- | ----- |
| **parcelid** | Unique identifier for parcels (lots) | Index/int |
| **heating_system_type_id** |  Type of home heating system  | int |
| **bathrooms** | Number of bathrooms in home including fractional bathrooms | float |
| **bedrooms** | Number of bedrooms in home | float |
| **prop_sqft** | Calculated total finished living area of the home | float |
| **fips** | County Code: 6037: LA County, 6059: Orange County, and 6111: Ventura County | int |
| **fireplace_cnt** | Number of fireplaces in a home (if any) | float |
| **latitude** | Latitude of the middle of the parcel multiplied by 10<sup>6</sup> | float |
| **longitude** | Longitude of the middle of the parcel multiplied by 10<sup>6</sup> | float |
| **lot_sqft** | Total Lot size in square feet | int |
| **pool_cnt** | Number of pools on the lot (if any) | float |
| **region_id_city** | City in which the property is located (if any) | int |
| **year_built** | The Year the principal residence was built | int |
| **fireplace_flag** | Is a fireplace present in this home | int |
| **struct_tax_value** | The assessed value of the built structure on the parcel | float |
| **tax_value** | The total tax assessed value of the parcel | float |
| **land_tax_value** | The assessed value of the land area of the parcel | float |
| **tax_amount** | The total property tax assessed for that assessment year | float |
| **log_error*** | Difference of the Zestimate and the actual transaction price | float |
| **heating_system_desc** | Type of home heating system description | object |
| **la_cnty** | Indicated if property is in LA County | uint8/binary |
| **orange_cnty** | Indicated if property is in Orange County | uint8/binary |
| **ventura_cnty** | Indicated if property is in Ventura County | uint8/binary|
| **log_error_class** | Binned data of log_error | category |
| **age** | year_built minus 2021 | int |
| **age_bin** | Bins the data of the age feature | float |
| **taxrate** | tax_amount divided by the tax_value | float |
| **acres** | lot_sqft divided by 43,560 | float |
| **structure_dollar_per_sqft** | struct_tax_value divided by prop_sqft | float |
| **structure_dollar_sqft_bin** | Binned data of structure_dollar_per_sqft | float |
| **land_dollar_per_sqft** | lot_sqft divided by land_tax_value | float |
| **lot_dollar_sqft_bin** | Binned data of land_dollar_per_sqft | float |
| **bath_bed_ratio** | Ratio of bathrooms to bedrooms | float |
| **cola** | City of Los Angeles | uint8/binary |

\* - Indicates the target feature in this Zillow data.

***

## <a name="wrangle"></a>Data Acquisition and Preparation
[[Back to top](#top)]

### Acquisition and Preparation
- Created **wrangle.py** file to handle the Aquisition and the Preparation of the Zillow Dataset

#### **wrangle.py** Functions for Acquisition and Preparation
| Function Name | Purpose |
| ----- | ----- | 
| **get_connection**(db, user=user, host=host, password=password) | This function uses my info from my env file to create a connection url to access the Codeup db. |
| **new_zillow_data**() | This function reads the zillow data from the Codeup db and returns a pandas DataFrame with all columns. |
| **get_zillow_data**(cached=False) | This function reads in zillow data from Codeup database and writes data to a csv file if cached == False or if cached == True reads in zillow df from a csv file, returns df |
| **wrangle_zillow**(cached=False) | This function if cached == False creates a dataframe from the zillow dataset and returns a cleaned and imputed version of the dataframe and if cached == True reads the wrangled_zillow.csv file |
| **handle_missing_values**(df, prop_to_drop_col, prop_to_drop_row) | This function takes in a dataframe, a number between 0 and 1 that represents the proportion, for each column, of rows with non-missing values required to keep the column, a another number between 0 and 1 that represents the proportion, for each row, of columns/variables with non-missing values required to keep the row, and returns the dataframe with the columns and rows dropped as indicated. |
| **create_features**(df) | Creates the folowing features: age, age_bin, taxrate, acres, acres_bin, sqft_bin, structure_dollar_per_sqft, structure_dollar_sqft_bin, land_dollar_per_sqft, lot_dollar_sqft_bin, bath_bed_ratio, and cola |

### Wrangle steps: 
- Takes in data from Codeup SQL server
- Remove rows with nulls in latitude and longitude
- Isolated Single Unit Properties
    - dropped properties where propertylandusetypeid == 246, 247, 248, 269
- Fireplace
    - If "fireplaceflag" is "True" and "fireplacecnt" is "NaN", we will set "fireplacecnt" equal to the median value of "1".
    - If 'fireplacecnt' is "NaN", replace with "0"
    - If "fireplacecnt" is 1 or larger "fireplaceflag" is "NaN", we will set "fireplaceflag" to "True".
    - Convert "True" to 1
- NULLs in poolcnt = No pools
- Removing column s and rows outside specifies threshhold
- Fill unitcnt NULLs with the mode (1) and delete the rest.
- Dropped null from the property value features
- Fill in beds and baths nulls with median because the range of values is small
- Convert to int and replace NULLs with mode for 'heatingorsystemdesc','heatingorsystemtypeid
- Convert latitude and longitude to positonal data points using lambda funtion (i.e. putting a decimal in the correct place)
- Impute kNN, for lotsizesquarefeet and regionidcity using 'latitude', 'longitude', 'fips', 'calculatedfinishedsquarefeet' and 'taxvaluedollarcnt' with 5 neighbors
- changed fips to an int
- Dummy FiPs
- Dropped columns not being used
- Binned log error column into 4 sections
- Rename columns for legibility
- Set index to parcelid
- Drop remaining null values
- Remove outliers of tax_value and lot_sqft
- Ran create features function
- Dropped remaining null values again
- Writes dataframe into a csv named "wrangled_zillow.csv"

*********************

## <a name="explore"></a>Data Exploration:
[[Back to top](#top)]
- Python files used for exploration:
    - wrangle.py 
    - explore.py
    - modeling.py

### **wrangle.py** Functions for Exploration
| Function Name | Definition |
| ------------ | ------------- |
| **train_validate_test_split**() | This function takes in a dataframe, the target feature as a string, and a seed interger and returns split data: train, validate, test, X_train, y_train, X_validate, y_validate, X_test, y_test |
| **train_validate_test_scale**(train, validate, test, quant_vars) | This function takes in the split data: train, validate, and test along with a list of quantitative variables, and returns scaled data for exploration and modeling |

### Pre-exploration:

Data features are separated into specific lists for use in the exploration process:
- binary_vars
    - pool_cnt
    - fireplace_flag
    - la_cnty
    - orange_cnty
    - ventura_cnty
    - cola
    
    
- categorical_vars
    - heating_system_type_id
    - fips
    - fireplace_cnt
    - heating_system_desc
    - region_id_city
    - acres_bin
    - sqft_bin
    - structure_dollar_sqft_bin
    
    
- quant_vars
    - bathrooms
    - bedrooms
    - prop_sqft
    - lot_sqft
    - year_built
    - struct_tax_value
    - tax_value
    - land_tax_value
    - tax_amount
    - structure_dollar_per_sqft
    - bath_bed_ratio


- categorical_target
    - log_error_class
    
    
- continuous_target
    - log_error

### **explore.py** Functions for Exploration
| Function Name | Definition |
| ------------ | ------------- |
| **explore_univariate**(train, categorical_vars, quant_vars) | This function makes use of the explore_univariate_categorical and explore_univariate_quant functions. It takes in a dataframe and a categorical variable and returns a frequency table and barplot of the frequencies, for a given categorical variable, compute the frequency count and percent split and return a dataframe of those values along with the different classes, and takes in a dataframequantitative variable and returns descriptive stats table, histogram, and boxplot of the distributions. |
| **explore_bivariate**(train, categorical_target, continuous_target, binary_vars, quant_vars) | This function makes use of explore_bivariate_categorical and explore_bivariate_quant functions. Each of those take in a continuous target and a binned/cut version of the target to have a categorical target. The categorical function takes in a binary independent variable and the quant function takes in a quantitative independent variable.  |

### **modeling.py** Functions for Exploration
| Function Name | Definition |
| ------------ | ------------- |
| **create_cluster**(train, X, k) | This function takes in train, X (dataframe with variables you want to cluster on) and k, It scales the X, calcuates the clusters and return train (with clusters), the Scaled dataframe, the scaler and kmeans object and unscaled centroids as a dataframe |
| **create_scatter_plot**(x,y,train,kmeans, X_scaled, scaler) |  This function takes in x and y (variable names as strings, along with returned objects from previous function create_cluster and creates a plot |

### Takeaways explore_univariate:
- Features with a lot of outliers:
    - bathrooms, prop_sqft, lot_sqft, struct_tax_value, tax_value, land_tax_value, tax_amount, and log_error
- Bedrooms appear to be normally distributed
- Latitude and longitude are good indicators of population density by location, it should not be scaled if being used by Geographical libraries

### Takeaways from explore_bivariate:
- Not one feature alone has a strong correlation with log_error
    - log_error has the strongest correlation with prop_sqft than the rest of the features
- log_error is highest in Orange county
- log_error is roughly the same in Los Angeles and Ventura (slightly less in Ventura).
- T-test resulted in rejecting the null hypothesis meaning we can assume that there is a difference in logerror between counties
- Results show logerror is significantly different for properties in LA County vs Orange County vs Ventura County
- slight upward trend in log error as square feet increases
- homes between 1,000-4,000 sq ft have quite a bit of variance in log error
- Grouping features with clustering should be a better route for predicting log_error
- Variance is lessens as the property value increases. 

### Clustering with: 
#### **<span style="color:green">from</span> sklearn.cluster <span style="color:green">import</span>  KMeansKMeans**

#### acres and age

- Created 4 clusters
    - young property low-mid acrage - new residential suburban areas 
    - mid century property low-mid acrage - mid century suburban and urban
    - old property low-mid acrage - old  suburban and urban
    - high acrage - all ages suburban-rural areas 

#### latitude and longitude

- Created 4 clusters:
    - Clusters follow the general county lines with the exception of cluster in the East.
    - County and location is a strong clustering feature 

#### tax_value and prop_sqft

- Created 4 clusters
    - low tax, low sqft - low end suburbs - lower middle class
    - mid tax, low sqft - mid-high end suburbs / low-mid end city - mid-high upper middle class
    - high tax, low sqft - high end suburbs and city property - upper middle class+
    - high sqft - large houses in all tax groups
    
***

## <a name="stats"></a>Statistical Analysis
[[Back to top](#top)]

### Stats Test 1: ANOVA Test: One Way

Analysis of variance, or ANOVA, is a statistical method that separates observed variance data into different components to use for additional tests. 

A one-way ANOVA is used for three or more groups of data, to gain information about the relationship between the dependent and independent variables: in this case our clusters vs. the log_error, respectively.

To run the ANOVA test in Python use the following import: \
<span style="color:green">from</span> scipy.stats <span style="color:green">import</span> f_oneway

- f_oneway, in this case, takes in the individual clusters and returns the f-statistic, f, and the p_value, p:
    - the f-statistic is simply a ratio of two variances. 
    - The p_vlaue is the probability of obtaining test results at least as extreme as the results actually observed, under the assumption that the null hypothesis is correct

#### Hypothesis:
- The null hypothesis (H<sub>0</sub>) is there is no difference in logerror between the 4 clusters.
- The alternate hypothesis (H<sub>1</sub>) is the log_error is different between the 4 clusters.

#### Confidence level and alpha value:
- I established a 95% confidence level
- alpha = 1 - confidence, therefore alpha is 0.05

#### Results:
- f = **10.3375**
- p-value = **8.5052 x 10<sup>-7</sup>**
- We reject the null hypothesis that log_error is the same across all 4 clusters
- We move forward with the alternative hypothesis that The log_error is different in the clusters

#### Summary:
- This indicates that the chosen features are a good start for modeling.


### Stats Test 2: T-Test: One Sample, Two Tailed
- A T-test allows me to compare a categorical and a continuous variable by comparing the mean of the continuous variable by subgroups based on the categorical variable
- The t-test returns the t-statistic and the p-value:
    - t-statistic: 
        - Is the ratio of the departure of the estimated value of a parameter from its hypothesized value to its standard error. It is used in hypothesis testing via Student's t-test. 
        - It is used in a t-test to determine if you should support or reject the null hypothesis
        - t-statistic of 0 = H<sub>0</sub>
    -  - the p-value:
        - The probability of obtaining test results at least as extreme as the results actually observed, under the assumption that the null hypothesis is correct
- We wanted to compare the individual clusters to the total population. 
    - Cluster1 to the mean of ALL clusters
    - Cluster2 to the mean of ALL clusters, etc.

#### Hypothesis:
- The null hypothesis (H<sub>0</sub>) is no difference in the means of the clusters' log_errors. 
- The alternate hypothesis (H<sub>1</sub>) is a significant difference in the means of the clusters' log_errors. 

#### Confidence level and alpha value:
- I established a 95% confidence level
- alpha = 1 - confidence, therefore alpha is 0.05


#### Results:
| Cluster | t-statistic | p-value | Reject Null? |
| ------- | -------: | -------: | :-------: |
| Cluster0 | -2.4378 | 0.0148 | Yes |
| Cluster1 | -4.4752 | 7.7 x 10<sup>-6</sup> | Yes |
| Cluster2 | 2.7658 | 0.0057 | Yes |
| Cluster3 | 1.2522 | 0.2106 | No |

#### Summary:
- Clusters 0, 1, and 2 reject the null hypothesis and are good candidates for modeling
- Cluster 3 failed to reject the null hypothesis and are not suitable for modeling.
***

## <a name="model"></a>Modeling:
[[Back to top](#top)]

### Model Preparation:
- Created dummy columns of the clusters in the train, validate and test dataframes
- Dropped the cluster that did not pass the t-test, cluster_3
- Setup X_train, y_train, X_validate, y_validate, X_test, and y_test 

### Baseline
    
```json
{
#Convert y_train and y_validate to dataframes to append the new columns with predicted values 
y_train = pd.DataFrame(y_train)
y_validate = pd.DataFrame(y_validate)

#Predict log_error_pred_mean
log_error_pred_mean = y_train['log_error'].mean()
y_train['log_error_pred_mean'] = log_error_pred_mean
y_validate['log_error_pred_mean'] = log_error_pred_mean

#Compute log_error_pred_median
log_error_pred_median = y_train['log_error'].median()
y_train['log_error_pred_median'] = log_error_pred_median
y_validate['log_error_pred_median'] = log_error_pred_median

#Evaluate the baseline model using the mean
#RMSE of log_error_pred_mean
rmse_train = mean_squared_error(y_train.log_error, y_train.log_error_pred_mean) ** .5
rmse_validate = mean_squared_error(y_validate.log_error, y_validate.log_error_pred_mean) ** .5

print("RMSE using Mean\nTrain/In-Sample: ", round(rmse_train, 4), 
      "\nValidate/Out-of-Sample: ", round(rmse_validate, 4))

#Evaluate the baseline model using the median
#RMSE of log_error_pred_mean
rmse_train = mean_squared_error(y_train.log_error, y_train.log_error_pred_median) ** .5
rmse_validate = mean_squared_error(y_validate.log_error, y_validate.log_error_pred_median) ** .5

print("RMSE using Median\nTrain/In-Sample: ", round(rmse_train, 4), 
      "\nValidate/Out-of-Sample: ", round(rmse_validate, 4))
}
```

- Baseline Results: 
    - RMSE using Median
    - Train/In-Sample:  0.1557 
    - Validate/Out-of-Sample:  0.1674        

- Selected features to input into models:
    - features = ['prop_sqft','tax_value','acres','age','latitude','bathrooms']

***

### Models and R<sup>2</sup> Values:
- Will run the following regression models:
    - Linear Regression
    - LassoLars Model
    - Tweedie Regressor (GLM)
    - Quadratic Regression
    

- Other indicators of model performance with breif defiition and why it's important:
    - R<sup>2</sup> Value is the coefficient of determination, pronounced "R squared", is the proportion of the variance in the dependent variable that is predictable from the independent variable. 
    - Essentially it is a statistical measure of how close the data are to the fitted regression line.
    
    
#### Model 1: Linear Regression (OLS)

```json 
{
# create the model object
lm = LinearRegression(normalize=True)

# fit the model to our training data
lm.fit(X_train[features], y_train.log_error)

# predict train
y_train['log_error_pred_lm'] = lm.predict(X_train[features])

# evaluate: rmse
rmse_train = mean_squared_error(y_train.log_error, y_train.log_error_pred_lm) ** (1/2)


# predict validate
y_validate['log_error_pred_lm'] = lm.predict(X_validate[features])

# evaluate: rmse
rmse_validate = mean_squared_error(y_validate.log_error, y_validate.log_error_pred_lm) ** (1/2)

print("RMSE for OLS using LinearRegression\nTraining/In-Sample: ", rmse_train, 
      "\nValidation/Out-of-Sample: ", rmse_validate)
}
```
- Model 1 results:
    - RMSE for OLS using LinearRegression
        - Training/In-Sample:  0.1552
        - Validation/Out-of-Sample:  0.1667
    - R<sup>2</sup> Value = 0.00214


### Model 2 : Lasso Lars Model

```json 
{
# create the model object
lars = LassoLars(alpha=1)

# fit the model to our training data
lars.fit(X_train[features], y_train.log_error)


# predict train
y_train['log_error_pred_lars'] = lars.predict(X_train[features])

# evaluate: rmse
rmse_train = mean_squared_error(y_train.log_error, y_train.log_error_pred_lm) ** (1/2)


# predict validate
y_validate['log_error_pred_lars'] = lars.predict(X_validate[features])

# evaluate: rmse
rmse_validate = mean_squared_error(y_validate.log_error, y_validate.log_error_pred_lm) ** (1/2)

print("RMSE for Lasso + Lars\nTraining/In-Sample: ", rmse_train, 
      "\nValidation/Out-of-Sample: ", rmse_validate)
}
```
- Model 2 results:
    - RMSE for Lasso + Lars
        - Training/In-Sample:  0.1552 
        - Validation/Out-of-Sample:  0.1667
    - R<sup>2</sup> Value = 2.2204 x 10<sup>-16</sup>

### Model 3 : Tweedie Regressor (GLM)

```json
{
# create the model object
glm = TweedieRegressor(alpha=0)

# fit the model to our training data. 
glm.fit(X_train[features], y_train.log_error)


# predict train
y_train['log_error_pred_glm'] = glm.predict(X_train[features])

# evaluate: rmse
rmse_train = mean_squared_error(y_train.log_error, y_train.log_error_pred_glm) ** (1/2)


# predict validate
y_validate['log_error_pred_glm'] = glm.predict(X_validate[features])

# evaluate: rmse
rmse_validate = mean_squared_error(y_train.log_error, y_train.log_error_pred_glm) ** (1/2)

print("RMSE for GLM using Tweedie, power=1 & alpha=0\nTraining/In-Sample: ", rmse_train, 
      "\nValidation/Out-of-Sample: ", rmse_validate)
}
```
- Model 3 results:
    - RMSE for GLM using Tweedie, power=1 & alpha=0
        - Training/In-Sample:  0.1552
        - Validation/Out-of-Sample:  0.1552
    - R<sup>2</sup> Value = 9.4674 x 10<sup>-4</sup>

### Model 4: Quadratic Regression Model
```json
{
## Polynomial Features ##
# make the polynomial features to get a new set of features
pf = PolynomialFeatures(degree=2)

# fit and transform X_train
X_train_degree2 = pf.fit_transform(X_train[features])

# transform X_validate & X_test
X_validate_degree2 = pf.transform(X_validate[features])
X_test_degree2 =  pf.transform(X_test[features])

## LinearRegression ##
# create the model object
lm2 = LinearRegression(normalize=True)

# fit the model to our training data
lm2.fit(X_train_degree2, y_train.log_error)

# predict train
y_train['log_error_pred_lm2'] = lm2.predict(X_train_degree2)

# evaluate: rmse
rmse_train = mean_squared_error(y_train.log_error, y_train.log_error_pred_lm2) ** (1/2)

# predict validate
y_validate['log_error_pred_lm2'] = lm2.predict(X_validate_degree2)

# evaluate: rmse
rmse_validate = mean_squared_error(y_validate.log_error, y_validate.log_error_pred_lm2)

print("RMSE for Polynomial Model, degrees=2\nTraining/In-Sample: ", rmse_train, 
      "\nValidation/Out-of-Sample: ", rmse_validate)
}
```
- Model 4 results:
    - RMSE for Polynomial Model, degrees=2
        - Training/In-Sample:  0.1550
        - Validation/Out-of-Sample:  0.0278
    - R<sup>2</sup> Value = 2.4659 x 10<sup>-3</sup>

## Selecting the Best Model:

### Use Table below as a template for all Modeling results for easy comparison:

| Model | Validation/Out of Sample RMSE | R<sup>2</sup> Value |
| ---- | ----| ---- |
| Baseline | 0.167366 | 2.2204 x 10<sup>-16</sup> |
| Linear Regression (OLS) | 0.166731 | 2.1433 x 10<sup>-3</sup> |  
| Tweedie Regressor (GLM) | 0.155186 | 9.4673 x 10<sup>-4</sup>|  
| Lasso Lars | 0.166731 | 2.2204 x 10<sup>-16</sup> |  
| Quadratic Regression | 0.027786 | 2.4659 x 10<sup>-3</sup> |  


- Quadratic Regression model performed the best
     - The RMSE of the validation/out of sample had the largest reduction from the baseline.
     - Also, this model has the highest R<sup>2</sup> value

## Testing the Model
```json
{
y_test = pd.DataFrame(y_test)

# predict on test
y_test['log_error_pred_lm2'] = lm2.predict(X_test_degree2)

# evaluate: rmse
rmse_test = mean_squared_error(y_test.log_error, y_test.log_error_pred_lm2) ** (1/2)

print("RMSE for Quadratic Model\nOut-of-Sample Performance: ", round(rmse_test, 4))
}
```
- Model Testing Results
     - Out-of-Sample Performance:  **0.160752**
     - R <sup>2</sup> value = -3.9362 x 10<sup>-4</sup>

***

## <a name="conclusion"></a>Conclusion:
[[Back to top](#top)]

- Polynomial Regression (Quadratic) Model with specified features outperformed baseline and did better than the other models:
    - RMSE: 0.160752
    
    
- However, the R<sup>2</sup> value indicates that this model is not a good fit
    - R<sup>2</sup> value: -3.936171 x 10<sup>-04</sup>
    
    
- Our goal of identifying the drivers for errors in Zestimates by incorporating clustering methodologies helped, but not by much.

- With more time:
    - would like to find if there are better predictors of log error.
    - would like to explore 3-D clustering by adding latitude or acres.
    - explore less common features like a/c unit type and fireplaces.
    - would like to fill out the missing data so that there are even more data points to work with.

