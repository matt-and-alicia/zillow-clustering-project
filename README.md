# <a name="top"></a>TITLE - readme.md
![](http://zillow.mediaroom.com/image/Zillow_Wordmark_Blue_RGB.jpg)

by: Alicia Gonzalez and Matthew Dalton
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

### Projet Outline:

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

- Is log error  significantly different for properties in LA County vs Orange County vs Ventura County?
- Is there a correlation between square footage of a home and log error?
- Is there a relationship between tax value and log error?
- Could there be clusters in square footage and county location that can help predict log error? 

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

- 


***

## <a name="findings"></a>Key Findings:
[[Back to top](#top)]

### Explore:
- What are your key findings from explore?


### Stats
- What are your key findings from stats?

### Modeling:
- Model results?


***

## <a name="dictionary"></a>Data Dictionary  
[[Back to top](#top)]

### Data Used
---
| Attribute | Definition | Data Type |
| ----- | ----- | ----- |
| parcelid | Unique identifier for parcels (lots) | Index/int |
| heating_system_type_id |  Type of home heating system  | int |
| bathrooms | Number of bathrooms in home including fractional bathrooms | float |
| bedrooms | Number of bedrooms in home | float |
| prop_sqft | Calculated total finished living area of the home | float |
| fips | County Code: 6037: LA County, 6059: Orange County, and 6111: Ventura County | int |
| fireplace_cnt | Number of fireplaces in a home (if any) | float |
| latitude | Latitude of the middle of the parcel multiplied by 10<sup>6</sup> | float |
| longitude | Longitude of the middle of the parcel multiplied by 10<sup>6</sup> | float |
| lot_sqft | Total Lot size in square feet | int |
| pool_cnt | Number of pools on the lot (if any) | float |
| region_id_city | City in which the property is located (if any) | int |
| year_built | The Year the principal residence was built | int |
| fireplace_flag | Is a fireplace present in this home | int |
| struct_tax_value | The assessed value of the built structure on the parcel | float |
| tax_value | The total tax assessed value of the parcel | float |
| land_tax_value | The assessed value of the land area of the parcel | float |
| tax_amount | The total property tax assessed for that assessment year | float |
| log_error* | Difference of the Zestimate and the actual transaction price | float |
| heating_system_desc | Type of home heating system description | object |
| la_cnty | Indicated if property is in LA County | uint8/binary |
| orange_cnty | Indicated if property is in Orange County | uint8/binary |
| ventura_cnty | Indicated if property is in Ventura County | uint8/binary|
| log_error_class | Binned data of log_error | category |
| age | year_built minus 2021 | int |
| age_bin | Bins the data of the age feature | float |
| taxrate | tax_amount divided by the tax_value | float |
| acres | lot_sqft divided by 43,560 | float |
| structure_dollar_per_sqft | struct_tax_value divided by prop_sqft | float |
| structure_dollar_sqft_bin | Binned data of structure_dollar_per_sqft | float |
| land_dollar_per_sqft | lot_sqft divided by land_tax_value | float |
| lot_dollar_sqft_bin | Binned data of land_dollar_per_sqft | float |
| bath_bed_ratio | Ratio of bathrooms to bedrooms | float |
| cola | City of Los Angeles | uint8/binary |

\* - Indicates the target feature in this Zillow data.

***

## <a name="wrangle"></a>Data Acquisition and Preparation
[[Back to top](#top)]

### Acquisition and Preparation
- Created **wrangle.py** file to handle the Aquisition and the Preparation of the Zillow Dataset
- 

| Function Name | Purpose |
| ----- | ----- | 
| get_connection(db, user=user, host=host, password=password) | This function uses my info from my env file to create a connection url to access the Codeup db. |
| new_zillow_data() | This function reads the zillow data from the Codeup db and returns a pandas DataFrame with all columns. |
| get_zillow_data(cached=False) | This function reads in zillow data from Codeup database and writes data to a csv file if cached == False or if cached == True reads in zillow df from a csv file, returns df |
| wrangle_zillow(cached=False) | This function if cached == False creates a dataframe from the zillow dataset and returns a cleaned and imputed version of the dataframe and if cached == True reads the wrangled_zillow.csv file |
| handle_missing_values(df, prop_to_drop_col, prop_to_drop_row) | This function takes in a dataframe, a number between 0 and 1 that represents the proportion, for each column, of rows with non-missing values required to keep the column, a another number between 0 and 1 that represents the proportion, for each row, of columns/variables with non-missing values required to keep the row, and returns the dataframe with the columns and rows dropped as indicated. |
| create_features(df) |

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

***

## <a name="explore"></a>Data Exploration:
[[Back to top](#top)]
- wrangle.py 

| Function Name | Definition |
| ------------ | ------------- |
| select_kbest | This function takes in a dataframe, the target feature as a string, and an interger (k) that must be less than or equal to the number of features and returns the (k) best features |
| rfe | This function takes in a dataframe, the target feature as a string, and an interger (k) that must be less than or equal to the number of features and returns the best features by making a model, removing the weakest feature, then, making a new model, and removing the weakest feature, and so on. |
| train_validate_test_split | This function takes in a dataframe, the target feature as a string, and a seed interger and returns split data: train, validate, test, X_train, y_train, X_validate, y_validate, X_test, y_test |
| get_object_cols() | This function takes in a dataframe and identifies the columns that are object types and returns a list of those column names |
| get_numeric_cols(X_train, object_cols) | This function takes in a dataframe and list of object column names and returns a list of all other columns names, the non-objects. |
| min_max_scale(X_train, X_validate, X_test, numeric_cols) | This function takes in 3 dataframes with the same columns, a list of numeric column names (because the scaler can only work with numeric columns), and fits a min-max scaler to the first dataframe and transforms all 3 dataframes using that scaler. It returns 3 dataframes with the same column names and scaled values. 


### Function1 used:
- Outcome of the use of the function 

### Function2 used:
- Outcome of the use of the function 

***

## <a name="stats"></a>Statistical Analysis
[[Back to top](#top)]

### Stats Test 1:
 - What is the test?
 - Why use this test?
 - What is being compared?

#### Hypothesis:
- The null hypothesis (H<sub>0</sub>) is... 
- The alternate hypothesis (H<sub>1</sub>) is ...


#### Confidence level and alpha value:
- I established a 95% confidence level
- alpha = 1 - confidence, therefore alpha is 0.05

#### Results:
 - Results of statistical tests

 - Summary:
     - In depth take-a-ways from the results

### Stats Test 2 
 - What is the test?
 - Why use this test?
 - What is being compared?

#### Hypothesis:
- The null hypothesis (H<sub>0</sub>) is... 
- The alternate hypothesis (H<sub>1</sub>) is ...


#### Confidence level and alpha value:
- I established a 95% confidence level
- alpha = 1 - confidence, therefore alpha is 0.05

#### Results:
 - Results of statistical tests

 - Summary:
     - In depth take-a-ways from the results

### Stats Test 3
 - What is the test?
 - Why use this test?
 - What is being compared?

#### Hypothesis:
- The null hypothesis (H<sub>0</sub>) is... 
- The alternate hypothesis (H<sub>1</sub>) is ...


#### Confidence level and alpha value:
- I established a 95% confidence level
- alpha = 1 - confidence, therefore alpha is 0.05

#### Results:
 - Results of statistical tests

 - Summary:
     - In depth take-a-ways from the results

***

## <a name="model"></a>Modeling:
[[Back to top](#top)]

Summary of modeling choices...

### Baseline


- What is the first step?
    
```json
{
Input code here if you want...
}
```
- Next Step:

```json
{
Code...
}
```

- Baseline Results: 
    - What are the numbers we are trying to beat with our model.
        
***

### Models and R<sup>2</sup> Values:
- Will run the following models:
    - Model 1
        - brief summary of what the model does.
    - Model 2 
        - brief summary of what the model does.
    - etc.

- Other indicators of model performance with breif defiition and why it's important:
    - R<sup>2</sup> Value is the coefficient of determination, pronounced "R squared", is the proportion of the variance in the dependent variable that is predictable from the independent variable. 
    - Essentially it is a statistical measure of how close the data are to the fitted regression line.
#### Model 1:

```json 
{
Model 1 code:
}
```
- Model 1 results:
    - Metric for Model 1:
        - Training/In-Sample:  **Results**
        - Validation/Out-of-Sample:  **Results**
    - Other metrics: (R<sup>2</sup> Value = )


### Model 2 :

```json 
{
Model 2 code:
}
```
- Model 2 results:
    - Metric for Model 1:
        - Training/In-Sample:  **Results**
        - Validation/Out-of-Sample:  **Results**
    - Other metrics: (R<sup>2</sup> Value = )


### Eetc:

## Selecting the Best Model:

### Use Table below as a template for all Modeling results for easy comparison:

| Model | Training/In Sample RMSE | Validation/Out of Sample RMSE | R<sup>2</sup> Value |
| ---- | ----| ---- | ---- |
| Baseline | 271194.48 | 272149.78 | -2.1456 x 10<sup>-5</sup> |
| Linear Regression | 217503.9051 | 220468.9564 | 0.3437 |
| Tweedie Regressor (GLM) | 217516.6069 | 220563.6468 | 0.3432 |
| Lasso Lars | 217521.8752 | 220536.3882 | 0.3433 |
| Polynomial Regression | 211227.5585 | 214109.6968 | 0.3810 |

- Why did you choose this model?
- 

## Testing the Model
```json
{
Model Testing Code...
}
```
- Model Testing Results
     - Out-of-Sample Performance:  **Results**


***

## <a name="conclusion"></a>Conclusion:
[[Back to top](#top)]

Reiterate explore findings, statistical analysis, and modeling take-a-ways

What could be done to improve the model?
What would you do with more time? 

Anything else of note worth adding? Add it here.

