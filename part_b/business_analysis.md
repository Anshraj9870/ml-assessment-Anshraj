# Business Case Analysis: Promotion Effectiveness at a Fashion Retail Chain

# B1. Problem Formulation

## (a) Machine Learning Problem

* **Target Variable:**
  `items_sold` (number of items sold per store per month)

* **Input Features:**

  * Promotion type (Flat Discount, BOGO, Free Gift, Category Offer, Loyalty Points)
  * Store attributes (store size, location type)
  * Customer behavior (footfall, basket size)
  * Competition density
  * Time-based features (month, weekends, festivals)

* **Problem Type:**
  This is a **Supervised Learning Regression Problem**

* **Justification:**
  The objective is to predict a continuous numerical value (`items_sold`). Since historical labeled data is available, regression is the most appropriate approach.

---

## (b) Why Items Sold Instead of Revenue

* Revenue is influenced by:

  * Pricing strategies
  * Discounts
  * Product mix

* `items_sold` directly reflects **customer demand and promotion effectiveness**.

* A promotion may increase sales volume but reduce revenue due to discounts.

### Broader Principle:

👉 The target variable should align closely with the **true business objective** and should not be distorted by external factors.

---

## (c) Alternative Modelling Strategy

Instead of using a single global model, a better approach is a **hybrid or hierarchical modelling strategy**:

* Train a global model using all data
* Incorporate **store-level features** (location, size, competition)
* Optionally segment stores into clusters (e.g., urban vs rural)

### Justification:

* Different stores respond differently to promotions
* Captures both **global patterns and local variations**
* Improves prediction accuracy and personalization

---

# B2. Data and EDA Strategy

## (a) Data Joining Strategy

### Data Sources:

* Transactions table
* Store attributes table
* Promotion details table
* Calendar table

### Joining Keys:

* `store_id`
* `transaction_date`

### Final Dataset Grain:

👉 **One row = one store per month**

### Aggregations:

* Total `items_sold` per store per month
* Total customer visits
* Average basket size
* Promotion applied during the month
* Festival and weekend indicators

This structured dataset ensures consistency and suitability for modelling.

---

## (b) Exploratory Data Analysis (EDA)

### 1. Promotion vs Sales (Bar Plot)

* Compare average items sold across promotions
* Helps identify high-performing promotions

### 2. Time Series Analysis

* Plot sales over time
* Detect seasonality (festivals, weekends)

### 3. Correlation Heatmap

* Identify relationships between features
* Helps in feature selection and avoiding multicollinearity

### 4. Store Segmentation Analysis

* Compare performance across store types (urban vs rural)
* Helps decide segmentation strategy

### Impact:

* Guides feature engineering
* Improves model selection
* Identifies trends and anomalies

---

## (c) Handling Data Imbalance

* Around 80% of data has no promotion

### Impact:

* Model may under-learn the effect of promotions
* Bias toward non-promotional scenarios

### Solution:

* Ensure sufficient representation of promotional data
* Create promotion-related features (promotion flag, type)
* Analyze promotion vs non-promotion cases separately
* Consider separate modelling strategies if needed

---

# B3. Model Evaluation and Deployment

## (a) Train-Test Split and Metrics

### Split Strategy:

* Use **time-based split**

  * Train: first 2.5 years
  * Test: last 6 months

### Why Not Random Split:

* Leads to **data leakage**
* Future information may influence training
* Produces unrealistic performance

---

### Evaluation Metrics:

* **RMSE (Root Mean Squared Error):**

  * Penalizes large errors
  * Measures prediction accuracy

* **MAE (Mean Absolute Error):**

  * Easy to interpret
  * Shows average prediction error

### Interpretation:

* Lower RMSE and MAE indicate better performance
* Helps assess reliability of predictions

---

## (b) Explaining Model Recommendations

To understand why different promotions are suggested:

* Use **feature importance** from Random Forest
* Analyze key drivers such as:

  * Month (seasonality)
  * Promotion type
  * Festival indicators

### Example:

* December → high demand → Loyalty Points works better
* March → lower demand → Flat Discount attracts customers

### Communication:

* Use simple charts and visuals
* Explain insights in business terms
* Avoid technical jargon

---

## (c) Deployment Strategy

### 1. Model Saving

```python
import joblib
joblib.dump(model, 'model.pkl')
```

---

### 2. Monthly Prediction Workflow

* Collect latest store and promotion data
* Apply same preprocessing pipeline
* Load saved model and generate predictions

---

### 3. Automation

* Run pipeline at the start of each month
* Generate promotion recommendations for all stores

---

### 4. Monitoring and Maintenance

Track:

* Prediction vs actual sales
* RMSE trends over time

### Retraining Trigger:

* Significant drop in performance
* Change in customer behavior or seasonality

---

## Final Conclusion

Machine Learning enables the retailer to:

* Optimize promotion strategies per store
* Increase sales efficiency
* Make data-driven decisions

This approach transforms marketing from intuition-based to a scalable, intelligent system.
