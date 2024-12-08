# Power Outage Analysis
---
Dec 7, 2024

By Adam Simpson

## Introduction

This project investigates a dataset documenting major power outages in the  U.S. from January 2000 to July 2016. The dataset provides details about outages' causes, geographical locations, climatic conditions, economic impacts, and characteristics of the affected areas. 

 The data was accessed from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure (LASCI)

My analysis began with data cleaning and exploratory data analysis. I cleaned the dataset and conducted analysis to gain a foundational understanding of the information provided, including distributions, trends, and relationships between key variables. I then investigated and imputed missing values.

My research focused on identifying the main factors that influence the cost of a power outage. By understanding these influences, energy companies and policymakers can make informed decisions about resource allocation, infrastructure investments, and preventative measures to minimize financial impacts and improve overall resilience.

The data set contains 36 rows and over 1000 outages. For this analysis, I concentrated on a subset of columns directly relevant to understanding the causes and costs of power outages.

| **Column Name**           | **Description**                                                                              |
|---------------------------|----------------------------------------------------------------------------------------------|
| `YEAR`                    | Year an outage occurred.                                                                     |
| `MONTH`                   | Month an outage occurred.                                                                    |
| `U.S._STATE`              | State where the outage occurred.                                                             |
| `ANOMALY.LEVEL`           | Oceanic El Niño/La Niña index referring to cold and warm episodes by season.                 |
| `OUTAGE.START.DATE`       | Date the outage started.                                                                     |
| `OUTAGE.START.TIME`       | Time the outage started.                                                                     |
| `OUTAGE.RESTORATION.DATE` | Date power was restored to all customers.                                                    |
| `OUTAGE.RESTORATION.TIME` | Time power was restored to all customers.                                                    |
| `CAUSE.CATEGORY`          | Categories of events causing the major power outages.                                        |
| `OUTAGE.DURATION`         | Duration of the outage in minutes.                                                           |
| `CUSTOMERS.AFFECTED`      | Number of customers affected by the power outage.                                            |
| `RES.PRICE`               | Average residential electricity price (cents/kWh).                                           |
| `COM.PRICE`               | Average commercial electricity price (cents/kWh).                                            |
| `IND.PRICE`               | Average industrial electricity price (cents/kWh).                                            |
| `RES.SALES`               | Residential electricity sales (MWh).                                                         |
| `COM.SALES`               | Commercial electricity sales (MWh).                                                          |
| `IND.SALES`               | Industrial electricity sales (MWh).                                                          |
| `POPULATION`              | Population of the affected state.                                                            |
| `PC.REALGSP.STATE`        | Per capita real gross state product (GSP).                                                   |
| `PC.REALGSP.CHANGE`       | Change in per capita real gross state product (GSP).                                         |
| `RES.CUSTOMERS`           | Number of residential customers served.                                                      |
| `COM.CUSTOMERS`           | Number of commercial customers served.                                                       |
| `IND.CUSTOMERS`           | Number of industrial customers served.                                                       |

## **Data Cleaning and Exploratory Data Analysis**

The first step in this analysis was preparing the dataset for effective analysis. This involved selecting relevant columns, combining date and time information, handling missing values, and engineering new features.

#### **Steps Taken**

1. **Column Selection**:  
   I only kept columns related to the research, including:
   - `U.S._STATE`
   - `TOTAL.CUSTOMERS`
   - `ANOMALY.LEVEL`
   - `CLIMATE.CATEGORY`
   - `OUTAGE.START.DATE`
   - `OUTAGE.START.TIME`
   - `OUTAGE.RESTORATION.DATE`
   - `OUTAGE.RESTORATION.TIME`
   - `CAUSE.CATEGORY`
   - `OUTAGE.DURATION`
   - `CUSTOMERS.AFFECTED`
   - `RES.PRICE`, `COM.PRICE`, `IND.PRICE`
   - `RES.SALES`, `COM.SALES`, `IND.SALES`
   - `POPULATION`

2. **Feature Engineering**:  
   - Combined `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into a single `OUTAGE.START` column.
   - Combined `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into a single `OUTAGE.RESTORATION` column.
   - Created `TOTAL.CUSTOMERS` by summing `RES.CUSTOMERS`, `COM.CUSTOMERS`, and `IND.CUSTOMERS`.
   - Created a binned column, `ANOMALY_BIN`, to categorize `ANOMALY.LEVEL` into Low, Medium, and High bins.

3. **Handling Missing Values**:  
   - Used median imputation for numerical columns like `ANOMALY.LEVEL`.
   - Forward-filled missing values in time-related columns such as `OUTAGE.START` and `OUTAGE.RESTORATION`.
   - Replaced missing values in `CAUSE.CATEGORY.DETAIL` with "none".
   - Dropped rows with missing values in cost-related columns.

4. **Normalization**:  
   - Scaled numerical columns for better performance in modeling.

---

### **Preview of Cleaned Data**

| **U.S._STATE** | **TOTAL.CUSTOMERS** | **ANOMALY.LEVEL** | **CLIMATE.CATEGORY** | **IND.CUSTOMERS** | **OUTAGE.RESTORATION**   | **OUTAGE.START**        | **TOTAL.COST** |
|----------------|---------------------|-------------------|----------------------|-------------------|--------------------------|-------------------------|----------------|
| Minnesota      | 2.60e+06           | -0.3             | Normal              | 10673.0          | 2011-07-03 20:00:00     | 2011-07-01 17:00:00    | 608,669.51     |
| Minnesota      | 2.64e+06           | -0.1             | Normal              | 9898.0           | 2014-05-11 18:39:00     | 2014-05-11 18:38:00    | 490,402.27     |
| Minnesota      | 2.59e+06           | -1.5             | Cold                | 10150.0          | 2010-10-28 22:00:00     | 2010-10-26 20:00:00    | 425,496.19     |
| Minnesota      | 2.61e+06           | -0.1             | Normal              | 11010.0          | 2012-06-20 23:00:00     | 2012-06-19 04:30:00    | 531,584.73     |
| Minnesota      | 2.67e+06           | 1.2              | Warm                | 9812.0           | 2015-07-19 07:00:00     | 2015-07-18 02:00:00    | 622,406.07     |

---


## Exploratory Data Analysis

### Univariate Analysis

#### Number of Outages by Cause
First, I analyzed the frequency of outages by their cause to understand the primary drivers of power disruptions. The bar chart below shows that **severe weather** is the most common cause, followed by **equipment failure** and **intentional attacks**. 
- **Severe Weather:** Infrastructure needs better resistance to natural disasters.
- **Equipment Failure:** Indicates maintenance gaps.
- **Intentional Attacks:** Highlights to the importance of improving grid security.

If we can understand the cause and its effects on price, we can begin to understand the most beneficial next steps in recovering from outages.
<iframe src="assets/num_outages.html" width="800" height="600" frameborder="0"></iframe>

To further explore the financial implications of power outages, I analyzed the **distribution of total costs** across different causes. The box plot below provides insight into the variability of costs associated with each outage cause. This step was important to identify which causes are not only frequent but expensive.

#### Key Observations:
1. **Severe Weather:** 
   - Exhibits a wide range of costs with several high-cost outliers.
   - Reinforces that it is the most impactful cause.
2. **Fuel Supply Emergencies:**
   - High median costs compared to other categories, though less frequent.
   - Indicates significant disruptions to infrastructure when they occur.
3. **Intentional Attacks:**
   - Lower frequency but wide variability in costs, showing potential for high economic damage in specific cases.
4. **System Operability Disruption:**
   - Usually moderate costs but with a few notable outliers.


<iframe src="assets/dist_costs_cause.html" width="800" height="600" frameborder="0"></iframe>

---

### Bivariate Analysis

#### Summary of Average Prices and Total Sales by Cause Category
I next wanted to look at different cost predictors and cause categories. The summarized table below highlights the average electricity prices (cents/kWh) and total sales (MWh) for residential, commercial, and industrial sectors, grouped by the causes of outages:
- **Key Insights:**
  - `Fuel supply emergencies` have the highest average prices across all sectors, indicating economic strain during these events.
  - `Severe weather` leads to the highest total sales across all sectors, reflecting its widespread impact.
  - `Intentional attacks` also result in high total sales, highlighting their potential to disrupt energy consumption patterns.


| Cause Category                | Avg Res Price | Avg Com Price | Avg Ind Price | Total Res Sales | Total Com Sales | Total Ind Sales |
|-------------------------------|---------------|---------------|---------------|-----------------|-----------------|-----------------|
| Equipment Failure             | 11.67         | 10.32         | 7.63          | 3.23e+08        | 3.58e+08        | 2.00e+08        |
| Fuel Supply Emergency         | 14.79         | 12.69         | 8.62          | 2.82e+08        | 3.46e+08        | 1.71e+08        |
| Intentional Attack            | 12.13         | 10.09         | 7.17          | 1.06e+09        | 1.09e+09        | 7.27e+08        |
| Islanding                     | 13.23         | 11.77         | 8.77          | 2.36e+08        | 3.10e+08        | 1.51e+08        |
| Public Appeal                 | 10.85         | 9.20          | 6.70          | 4.64e+08        | 4.15e+08        | 2.72e+08        |
| Severe Weather                | 11.71         | 9.88          | 7.23          | 3.44e+09        | 3.36e+09        | 2.23e+09        |
| System Operability Disruption | 12.14         | 10.63         | 7.79          | 7.55e+08        | 8.11e+08        | 4.74e+08        |

### Summary of Prices and Sales by Climate Category

To gain insight into how climate conditions might influence electricity prices and sales, I created a **pivot table** to summarize the average residential, commercial, and industrial electricity prices and sales across different climate categories. This analysis aims to identify patterns in energy usage and pricing under varying climatic conditions.

#### Table of Results:


| Climate Category  | Avg Res Price | Avg Com Price | Avg Ind Price | Avg Res Sales | Avg Com Sales | Avg Ind Sales |
|-------------------|---------------|---------------|---------------|---------------|---------------|---------------|
| Cold              | 11.90         | 10.15         | 7.40          | 4.33e+06      | 4.31e+06      | 2.72e+06      |
| Normal            | 12.06         | 10.16         | 7.38          | 4.10e+06      | 4.23e+06      | 2.79e+06      |
| Warm              | 11.87         | 10.04         | 7.16          | 4.93e+06      | 5.08e+06      | 2.94e+06      |


#### Key Observations:
1. **Prices:**
   - The average residential price is highest in the "Normal" climate category (12.06 cents/kWh), slightly above "Cold" and "Warm".
   - Commercial and industrial prices are slightly lower in the "Warm" category compared to "Cold" and "Normal."

2. **Sales:**
   - Residential and commercial sales are highest in the "Warm" climate category, indicating increased energy usage during warmer conditions.
   - Industrial sales are also highest in the "Warm" category, potentially due to higher operational energy demands in warmer climates.

#### Analysis and Insights:
- Warmer climates seem to drive higher electricity consumption across all customer types, likely due to cooling demands.
- Prices remain relatively stable across climate categories, with only minor fluctuations observed.

This pivot table analysis complements the broader exploration of energy usage and pricing by highlighting potential regional and seasonal patterns, which are crucial for forecasting and policy-making.

#### Cost vs. Customers Affected
Next, I explored the relationship between the number of customers affected and the total cost of outages. I chose this analysis to identify how customer impact translates into financial loss. The scatter plot below reveals a strong positive relationship between these two variables, where larger outages tend to mean higher costs. 

<iframe src="assets/cost_vs_customers.html" width="800" height="600" frameborder="0"></iframe>

### Baseline Model Setup

To create a baseline for predicting the total cost of power outages, I used a **Linear Regression model**. This model provides a benchmark for understanding the relationships between the features and the response variable, `TOTAL.COST`.

### Features and Target Variable
The features and target variable selected for the baseline model:
- **Features:**
  - `U.S._STATE` (categorical)
  - `CUSTOMERS.AFFECTED` (numerical)
  - `CAUSE.CATEGORY` (categorical)
  - `ANOMALY.LEVEL` (numerical)
- **Target Variable:**
  - `TOTAL.COST` (numerical)

### Justification of Feature Selection
It is important to ensure that all features included in the model would be available at the time of prediction. In this case, they are:
- **`CUSTOMERS.AFFECTED`:** This metric is often reported during or shortly after an outage begins, making it realistic for prediction purposes. Its easy to estimate based on data that is always available.
- **`CAUSE.CATEGORY`:** This is an attribute assigned based on the nature of the outage, which is typically known early in the process.
- **`ANOMALY.LEVEL`:** This metric, derived from climate data, is known prior to or during the outage.
- **`U.S._STATE`:** The location of the outage.

By ensuring that features do not rely on future information such as total restoration time, the model aligns with real-world constraints.

---

### Model Description

For the final model, I implemented a **Random Forest Regressor**, a flexible and robust ensemble learning method that handles non-linear relationships and interactions between features. Random forests are well-suited for this prediction task due to their ability to handle both categorical and numerical data and avoid overfitting through bagging.

### Features in the Final Model
The following features were used:
1. **Quantitative Features (6):**
   - `CUSTOMERS.AFFECTED`
   - `ANOMALY.LEVEL`
   - `RES.PRICE` (Residential electricity price)
   - `COM.PRICE` (Commercial electricity price)
   - `IND.PRICE` (Industrial electricity price)
   - `TOTAL.CUSTOMERS` (Sum of residential, commercial, and industrial customers)
   
2. **Ordinal Features (1):**
   - `ANOMALY_BIN` (Binned anomaly levels into "Low," "Medium," and "High")

3. **Nominal Features (4):**
   - `U.S._STATE`
   - `CAUSE.CATEGORY`
   - `CAUSE.CATEGORY.DETAIL`
   - `CLIMATE.CATEGORY`

### Encoding and Data Preprocessing
- **Quantitative Features:** Imputed missing values with the median and scaled using `StandardScaler`.
- **Ordinal Features:** Encoded using integer mappings (e.g., "Low" = 0, "Medium" = 1, "High" = 2).
- **Nominal Features:** One-hot encoded using `OneHotEncoder` to convert categories into dummy variables.
The preprocessing ensured the model handled mixed data types effectively. Other data changes were inhereted from the baseline model.

---

### Feature Engineering
1. **`TOTAL.CUSTOMERS`:** Summed across residential, commercial, and industrial customers. This feature captures the total customer base impacted by the outage.
2. **`ANOMALY_BIN`:** Categorized the continuous `ANOMALY.LEVEL` feature into bins ("Low," "Medium," "High") to identify patterns in anomalies' impact on cost.

These features were selected based on their relevance to the data generating process. `TOTAL.CUSTOMERS` directly measures scale, while `ANOMALY_BIN` simplifies understanding of climate-related influences.

---

### Model Selection and Hyperparameter Tuning

The **Random Forest Regressor** was optimized using **GridSearchCV** with the following hyperparameter search space:
- **`n_estimators` (number of trees):** [100, 200, 300]
- **`max_depth` (maximum tree depth):** [10, 20, 30]
- **`max_features` (maximum features to consider):** [0.5, "sqrt", "log2"]
- **`min_samples_split` (minimum samples to split):** [2, 5, 10]
- **`min_samples_leaf` (minimum samples per leaf):** [1, 2, 4]

After 5-fold cross-validation, the best-performing hyperparameters were:
- `n_estimators`: 200
- `max_depth`: 30
- `max_features`: 0.5
- `min_samples_split`: 2
- `min_samples_leaf`: 1

The grid search optimized the model’s performance while preventing overfitting.

---

### Performance Metrics

**Baseline Model Performance (Linear Regression):**
- **MAE:** $196,893.50
- **RMSE:** $317,437.32

**Final Model Performance (Random Forest):**
- **MAE:** $30,012.54
- **RMSE:** $57,467.16

The final model's performance indicates a strong ability to predict outage costs accurately, driven by its ability to handle non-linearities and feature interactions.

The final model substantially outperforms the baseline model, reducing the RMSE by over 80%. This improvement reflects the Random Forest's ability to model complex relationships and capture the interactions of data in the outages data set

---

### Model Evaluation

The final model is decent at predicting costs.
1. It demonstrates a significant reduction in prediction errors compared to the baseline.
2. Cross-validation ensured robustness, reducing the likelihood of overfitting.
3. The errors in price are relatively small in scale to total costs of outages, which can be in the millions.

<iframe src="assets/model_performance.html" width="800" height="600" frameborder="0"></iframe>

---
