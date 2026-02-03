# Bank Marketing Campaign Analysis (CRISP-DM)

## Project Overview
This project applies machine learning techniques to improve the effectiveness of direct marketing campaigns for a Portuguese banking institution. Using the Bank Marketing dataset from the UCI Machine Learning Repository, the analysis focuses on identifying customers most likely to subscribe to a term deposit while addressing significant class imbalance.

The project follows the **CRISP-DM (Cross-Industry Standard Process for Data Mining)** framework to ensure a structured, business-driven approach.

---

## Business Understanding (CRISP-DM Phase 1)

### Business Problem
The bank conducts direct marketing campaigns to promote term deposits. Most customers do not subscribe, resulting in highly imbalanced data. A naïve strategy that contacts every customer is inefficient, costly, and risks customer fatigue.

### Business Objective
Improve campaign targeting by identifying customers who are more likely to subscribe, thereby:
- Increasing conversion rates
- Reducing unnecessary outreach
- Improving marketing efficiency

### Success Criteria
- Go beyond accuracy as the primary metric
- Prioritize **Recall** and **F1 score** for the positive class (“yes”)
- Demonstrate measurable improvement over a naïve baseline model

---

## Data Understanding (CRISP-DM Phase 2)

### Dataset
- **Source:** UCI Machine Learning Repository – Bank Marketing Dataset  
- **Observations:** ~41,000 customers  
- **Target Variable:**  
  - `y`: whether the client subscribed to a term deposit (`yes` / `no`)

### Marketing Campaigns
The dataset represents results from **17 direct marketing campaigns** conducted via phone calls.

### Key Characteristics
- Strong class imbalance (~11% positive class)
- Categorical-heavy feature set
- “Unknown” values used instead of NaN (by design)

---
## Exploratory Data Analysis (EDA)

The exploratory analysis focused on understanding the structure of the data, identifying potential data quality issues, and assessing factors that influence subscription behavior.

### Key Findings

- **Severe class imbalance**
  - Approximately 11% of customers subscribed to a term deposit.
  - A naïve model predicting all customers as “no” achieves high accuracy but provides no business value.

- **Customer demographics**
  - Age distributions differ slightly between subscribers and non-subscribers, with older customers showing marginally higher subscription rates.
  - Certain job categories (e.g., retired, student, management) exhibit higher relative subscription likelihood.

- **Categorical feature dominance**
  - Most input features are categorical, requiring encoding prior to modeling.
  - Several features include an explicit `"unknown"` category, indicating missing or undisclosed information rather than true null values.

- **Implicit missingness**
  - Missing data is represented using sentinel values (e.g., `"unknown"` or `pdays = 999`) instead of NaN.
  - These values were retained and treated as meaningful categories during modeling.

- **Limited predictive power of client attributes alone**
  - Bank client features provide some signal but are insufficient for strong subscriber prediction on their own.
  - This suggests the importance of incorporating campaign history and economic indicators in future modeling stages.

### EDA Implications for Modeling

- Accuracy is an unreliable metric due to class imbalance.
- Recall and F1 score are more appropriate for evaluating subscriber identification.
- Feature engineering and model tuning are required to extract meaningful performance from the data.

---

## Data Preparation (CRISP-DM Phase 3)

### Feature Selection
For baseline modeling, only **bank client attributes** were used:
- Age
- Job
- Marital status
- Education
- Credit default status
- Housing and personal loan indicators

### Encoding Strategy
- One-hot encoding for categorical variables
- Target encoded as binary (`yes = 1`, `no = 0`)
- “Unknown” values retained as valid categories

### Scaling
- Scaling initially skipped to preserve interpretability
- Applied later **only to numeric features** for distance-based models (KNN, SVM)

---

## Modeling (CRISP-DM Phase 4)

### Baseline Model
A naïve baseline predicts all customers as “no.”
- Accuracy ≈ 89%
- Recall (yes) = 0  
This establishes a realistic benchmark that meaningful models must outperform.

---

## Model Evaluation (CRISP-DM Phase 5)

### Default Model Comparison (Problem 10)
Default model configurations achieve high accuracy but perform poorly at identifying subscribers, confirming that accuracy alone is misleading.

**[INSERT FIGURE: Default Models – F1 Score Comparison]**  
`images/problem10_default_models_f1.png`

---

### Tuned Model Comparison (Problem 11)
After hyperparameter tuning and metric alignment, models show substantial improvement in recall and F1 score.

**[INSERT FIGURE: Tuned Models – F1 Score Comparison]**  
`images/problem11_tuned_models_f1.png`

---

### Performance vs. Cost Tradeoff
Compares predictive effectiveness (F1 score) against training time to assess operational feasibility.

**[INSERT FIGURE: Train Time vs F1 Scatter Plot]**  
`images/train_time_vs_f1_scatter.png`

---

## Model Improvement Summary (CRISP-DM Phase 6)

### Tuned Model Results (Test Set)

| Model | Recall (yes) | F1 (yes) | Train Time |
|-----|-------------|----------|-----------|
| Logistic Regression (tuned) | ~0.64 | ~0.25 | Fast |
| Linear SVM (tuned) | ~0.62 | ~0.25 | Moderate |
| Decision Tree (tuned) | ~0.92 | ~0.23 | Very fast |
| KNN (tuned) | ~0.10 | ~0.14 | Fast |

### Key Insights
- Metric alignment has a greater impact than algorithm choice
- Decision Trees maximize recall but sacrifice overall accuracy
- Logistic Regression offers the best balance of performance, interpretability, and efficiency

---

## Recommendations

- Use **Recall and F1 score** as primary KPIs for campaign targeting
- Deploy **tuned Logistic Regression** as the primary model
- Use Decision Trees when maximum recall is required
- Expand feature space to include campaign history and economic indicators
- Retrain models periodically to reflect changing customer behavior

---

