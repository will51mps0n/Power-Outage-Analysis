# Power Outage Analysis
---
---

# Power Outage Analysis

## Introduction

This project investigates a dataset documenting major power outages in the  U.S. from January 2000 to July 2016. The dataset provides details about outages' causes, geographical locations, climatic conditions, economic impacts, and characteristics of the affected areas. 

The dataset tracks significant power outages in the a single U.S. state at the time of occurrence. It was accessed from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure (LASCI)

The dataset provides detailed information about major outages, including financial breakdowns, demographics of customers affected, and geographic in

My analysis began with data cleaning and exploratory data analysis. I cleaned the dataset and conducted analysis to gain a foundational understanding of the information provided, including distributions, trends, and relationships between key variables. I then investigated and imputed missing values.

My research focuses on identifying the main factors that influence the cost of a power outage. By understanding these influences, energy companies and policymakers can make informed decisions about resource allocation, infrastructure investments, and preventative measures to minimize financial impacts and improve overall resilience.

The data set contains 36 rows and thousands of outages. For this analysis, I concentrated on a subset of columns directly relevant to understanding the causes and costs of power outages.

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

### **Data Cleaning**

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

### Research Question
**What factors most influence the cost of power outages, and how can these insights guide decision-making?**

### Dataset Summary
- **Rows**: 10,000
- **Columns**: 30
- **Relevant Columns**:
  - **`TOTAL.COST`**: Total financial impact of the outage (USD).
  - **`CAUSE.CATEGORY`**: The primary cause of the outage.
  - **`TOTAL.CUSTOMERS`**: Total customers affected.
  - **`ANOMALY.LEVEL`**: Severity of the anomaly (low, medium, high).

---

## Data Cleaning

The raw dataset contained various issues, including missing values and redundant columns. Cleaning steps included:

1. **Removed unnecessary columns**: Kept only columns relevant to the research question.
2. **Combined columns**: Created `TOTAL.CUSTOMERS` by summing residential, commercial, and industrial customers.
3. **Imputed missing values**:
   - Median imputation for `ANOMALY.LEVEL`.
   - Forward fill for datetime columns.
4. **Generated new features**:
   - `ANOMALY_BIN`: Binned `ANOMALY.LEVEL` into low, medium, and high categories.

### Cleaned Dataset Preview
| U.S._STATE | TOTAL.CUSTOMERS | ANOMALY.LEVEL | TOTAL.COST |
|------------|------------------|---------------|------------|
| Minnesota  | 2,600,000        | -0.3          | 608,669.51 |
| Minnesota  | 2,640,000        | -0.1          | 490,402.27 |
| Minnesota  | 2,590,000        | -1.5          | 425,496.19 |

---

## Exploratory Data Analysis

### Univariate Analysis

#### Number of Outages by Cause
First, I analyzed the frequency of outages by their cause to understand the primary drivers of power disruptions. The bar chart below shows that **severe weather** is the most common cause, followed by **equipment failure** and **intentional attacks**. Severe weather dominates due to its widespread and recurring nature, such as hurricanes, storms, and heatwaves. 

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
| Climate Category | Avg Res Price | Avg Com Price | Avg Ind Price | Avg Res Sales | Avg Com Sales | Avg Ind Sales |
|-------------------|---------------|---------------|---------------|---------------|---------------|---------------|
| Cold              | 11.90         | 10.15         | 7.40          | 4.33e+06      | 4.31e+06      | 2.72e+06      |
| Normal            | 12.06         | 10.16         | 7.38          | 4.10e+06      | 4.23e+06      | 2.79e+06      |
| Warm              | 11.87         | 10.04         | 7.16          | 4.93e+06      | 5.08e+06      | 2.94e+06      |

#### Key Observations:
1. **Prices:**
   - The average residential price is highest in the "Normal" climate category (12.06 cents/kWh), slightly above "Cold" and "Warm" categories.
   - Commercial and industrial prices are slightly lower in the "Warm" category compared to "Cold" and "Normal."

2. **Sales:**
   - Residential and commercial sales are highest in the "Warm" climate category, indicating increased energy usage during warmer conditions.
   - Industrial sales are also highest in the "Warm" category, potentially due to higher operational energy demands in warmer climates.

#### Analysis and Insights:
- Warmer climates seem to drive higher electricity consumption across all customer types, likely due to cooling demands and other seasonal factors.
- Prices remain relatively stable across climate categories, with only minor fluctuations observed, suggesting a more uniform pricing strategy regardless of climatic variations.

This pivot table analysis complements the broader exploration of energy usage and pricing by highlighting potential regional and seasonal patterns, which are crucial for forecasting and policy-making.

#### Cost vs. Customers Affected
Next, I explored the relationship between the number of customers affected and the total cost of outages. I chose this analysis to identify how customer impact translates into financial loss. The scatter plot below reveals a strong positive relationship between these two variables, where larger outages tend to mean higher costs. 

This highlights:
- **High-Impact Events:** Severe weather and fuel supply emergencies often cluster in the high-cost, high-impact range, showing their significant financial burden.
- **Variability in Costs:** Outages caused by intentional attacks show a wide range of costs despite affecting fewer customers, reflecting localized but costly impacts.

<iframe src="assets/cost_vs_customers.html" width="800" height="600" frameborder="0"></iframe>

---

## Model Pipeline

### Baseline Model
The baseline model used a **RandomForestRegressor** with basic features:
- **Features**:
  - Quantitative: `TOTAL.CUSTOMERS`, `CUSTOMERS.AFFECTED`.
  - Nominal: `CAUSE.CATEGORY`.
- **Performance**:
  - **MAE**: $50,000
  - **RMSE**: $70,000

---

### Final Model
## Model Pipeline: Random Forest Regressor

The **RandomForestRegressor** was chosen for its robustness in capturing complex relationships within the dataset. This ensemble method is particularly well-suited for data with both numerical and categorical features, as well as non-linear interactions.

### Key Hyperparameters Tuned

To optimize the model's performance, the following hyperparameters were tuned using GridSearchCV:

- **`n_estimators`**: 200  
  Represents the number of trees in the forest. Increasing this value improves stability but also increases computational cost.
  
- **`max_depth`**: 30  
  Limits the depth of each decision tree to prevent overfitting while capturing sufficient complexity.
  
- **`max_features`**: 0.5  
  Specifies the fraction of features considered for splitting at each node, balancing accuracy and diversity among the trees.

These hyperparameters were fine-tuned based on 5-fold cross-validation to ensure optimal performance on unseen data.

---

## Model Performance

The final model demonstrated strong predictive capabilities on the test set, effectively identifying patterns and trends across key features. Performance metrics included:

- **Mean Absolute Error (MAE)**: $30,178.28  
  Reflects the average absolute deviation of predictions from actual values.

- **Root Mean Squared Error (RMSE)**: $56,496.68  
  Emphasizes larger prediction errors, indicating the presence of some outliers that require further investigation.

### Strengths:
- Successfully captures the relationships between key features such as `COM.SALES` and `POPULATION` with the target variable, `TOTAL.COST`.
- Robust against overfitting due to hyperparameter tuning.

### Challenges:
- Outliers in the data introduce variability in predictions, slightly increasing the RMSE compared to the MAE.

- **Predicted vs Actual Costs**:
<iframe src="assets/model_performance.html" width="800" height="600" frameborder="0"></iframe>

---

## Next Steps

To further refine the model:
1. Investigate and address the outliers to reduce prediction errors.
2. Experiment with alternative algorithms, such as Gradient Boosting or XGBoost, for potential performance gains.
3. Incorporate additional features, such as regional economic activity or weather data, to improve model robustness.

The current pipeline serves as a strong baseline for understanding the cost drivers of power outages and can be expanded with additional data or refined for specific use cases.

This analysis revealed that the cost of power outages is heavily influenced by commercial sales and population density. By leveraging a tuned Random Forest model, we improved prediction accuracy significantly, providing actionable insights for resource planning.
