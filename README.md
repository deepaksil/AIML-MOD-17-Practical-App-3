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

Model performance was evaluated using both training and test data to assess generalization, overfitting, and alignment with the business objective of identifying subscribers. Due to significant class imbalance, accuracy alone was not used as the primary success metric.

### Default Model Performance (Problem 10)

Using default model configurations, all evaluated algorithms achieved high test accuracy (approximately 86–89%). However, this performance was driven almost entirely by the majority class (“no”), resulting in poor identification of subscribing customers.

Key observations from default models include:

- **Logistic Regression** and **SVM** generalized well in terms of accuracy but failed to identify any subscribers, yielding zero recall and F1 score for the positive class.
- **Decision Tree** achieved near-perfect training accuracy, indicating overfitting, but showed limited recall on the test set.
- **KNN** identified a small number of subscribers but suffered from low recall and limited overall usefulness.

These results confirm that accuracy is a misleading metric for this problem and motivate the need for metric realignment and model tuning.

---

### Improved Model Performance (Problem 11)

To address the shortcomings of default models, hyperparameter tuning and metric adjustment were applied. Models were optimized using recall and F1 score for the positive class to better align with the business objective.

Key findings from tuned models include:

- **Logistic Regression (tuned)** achieved the best balance between recall, F1 score, stability, and computational efficiency.
- **Decision Tree (tuned)** maximized recall by aggressively identifying subscribers but did so at the cost of reduced accuracy and precision.
- **Linear SVM (tuned)** produced comparable recall and F1 scores to Logistic Regression but required more computational effort.
- **KNN (tuned)** showed limited improvement and remained less effective relative to other models.

Overall, tuning and metric alignment significantly improved model effectiveness, demonstrating that optimization strategy had a greater impact on performance than algorithm complexity.

---

### Evaluation Summary

- Training accuracy helped identify overfitting behavior, particularly in Decision Trees.
- Test-set recall and F1 score provided the most meaningful measures of model success.
- Simpler, well-tuned models outperformed more complex alternatives when evaluated against business-aligned criteria.

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

