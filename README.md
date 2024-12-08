# Power_Outages_Analysis
---
layout: page
title: Power Outage Analysis
---

# Power Outage Analysis

## Introduction

The dataset focuses on power outages across the United States, including information about outage causes, durations, affected customers, and associated costs. 

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
The bar chart below highlights the frequency of outages by cause category. Severe weather dominates, suggesting it plays a significant role in outage frequency.

<iframe src="assets/num_outages.html" width="800" height="600" frameborder="0"></iframe>

---

### Bivariate Analysis

#### Cost vs. Customers Affected
The scatter plot below reveals a strong positive relationship between the number of customers affected and the total cost of outages. Larger outages tend to result in higher costs.

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
