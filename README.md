# Revenue Classification Project

## Introduction
This project addresses the challenge of predicting whether a user session will result in revenue generation (*Revenue* vs *No Revenue*).  
The dataset includes various session-level features such as the number of administrative, informational, and product-related pages viewed, along with durations, bounce rates, exit rates, and visit characteristics.  

The main objectives of this project were:
- To design and implement new feature engineering techniques that better capture user intent and browsing behavior.
- To handle the issue of class imbalance in the dataset.
- To evaluate and compare multiple machine learning models.
- To identify the most robust and generalizable model for deployment.

---

## Libraries Used

- **pandas** – Data manipulation, cleaning, and feature creation.  
- **numpy** – Numerical computations and mathematical transformations.  
- **scikit-learn (sklearn)** – Machine learning utilities such as train/test splits, model building, and evaluation.  
- **imbalanced-learn (imblearn)** – Techniques designed to address imbalanced datasets.  

### Specialized Techniques and Models

- **BalancedRandomForestClassifier**  
  A Random Forest variant that ensures balanced class representation by sampling equally from each class during training.

- **EasyEnsembleClassifier**  
  An ensemble method that trains multiple AdaBoost classifiers on balanced bootstrap samples to improve performance on minority classes.

- **SMOTE (Synthetic Minority Oversampling Technique)**  
  Generates synthetic examples of the minority class by interpolating between existing samples.

- **SMOTENC (SMOTE for Nominal and Continuous features)**  
  An extension of SMOTE that supports datasets with both categorical and continuous variables.

---

## Feature Engineering

Several new features were engineered to enhance predictive performance. These features are designed to capture browsing behavior, user efficiency, and intent:

- **Entropy-based measures**  
  - `pageTypeEntropy`: Captures the randomness in page types visited.  
  - `timeShareEntropy`: Captures the randomness in time spent across page types.  

- **Dominant page type**  
  Identifies the primary type of page visited (Administrative, Informational, or Product).

- **Distinct page types touched**  
  Counts the number of unique page types visited within a session.

- **Efficiency and intent measures**  
  - `viewsPerMinute`, `valuePerSecond`, `avgTimeRatio`, `productVSNonProductAvgTimeRatio`.  
  - Indicate the efficiency of browsing and potential purchase intent.

- **Bounce and exit behavior**  
  - `bounceExitRatio`, `exitPenalizedValue`, `highBounceLowFocus`.  
  - Capture likelihood of drop-off or low engagement.

- **Calendar and cohort interactions**  
  - Features related to `specialDay`, `weekend`, `returningVisitor`, `holidaySeason`, and `quarterEnd`.

- **Composite indices**  
  - `intentIndex`: Combines product focus, bounce, exit, and value/time ratios.  
  - `frictionIndex`: Measures browsing effort relative to bounce behavior and duration.

These engineered features provided significant improvements in capturing user behavior and intent.

---

## Models Used

The following models were implemented and compared:

1. Logistic Regression – Baseline linear model.  
2. Gradient Boosting Classifier – Tree-based boosting approach.  
3. CatBoost Classifier – Gradient boosting method optimized for categorical data.  
4. BalancedRandomForestClassifier – Designed to handle imbalanced datasets effectively.  
5. EasyEnsembleClassifier – An ensemble method for severe imbalance scenarios.  

---

## Model Evaluation

Among the tested models, CatBoost showed the most robust and generalizable results.

### Validation (Train) Performance

| Metric        | No Revenue | Revenue | Macro Avg | Weighted Avg |
|---------------|------------|---------|-----------|--------------|
| Precision     | 0.95       | 0.63    | 0.79      | 0.90         |
| Recall        | 0.92       | 0.75    | 0.84      | 0.89         |
| F1-score      | 0.94       | 0.68    | 0.81      | 0.90         |
| Accuracy      | -          | -       | **0.89**  | -            |

**Support:**  
- No Revenue: 8,338 samples  
- Revenue: 1,526 samples  
- Total: 9,864 samples  

---

### Test Performance

| Metric        | No Revenue | Revenue | Macro Avg | Weighted Avg |
|---------------|------------|---------|-----------|--------------|
| Precision     | 0.95       | 0.61    | 0.78      | 0.90         |
| Recall        | 0.92       | 0.72    | 0.82      | 0.89         |
| F1-score      | 0.93       | 0.66    | 0.80      | 0.89         |
| Accuracy      | -          | -       | **0.89**  | -            |


---

### Observations

- Both train and test accuracies are consistent at 89%, indicating minimal overfitting.  
- ROC AUC of 0.92 shows strong separation between classes.  
- The model is very reliable for predicting *No Revenue* sessions.  
- For *Revenue* sessions, recall (72%) is reasonably strong, but precision (61%) suggests some false positives remain.  
- CatBoost clearly outperformed baseline logistic regression and other ensemble methods in terms of generalization and stability.

---

## Conclusion

- Feature engineering was critical to improving predictive performance.  
- Class imbalance was managed using both data-level (SMOTE, SMOTENC) and model-level (Balanced RF, Easy Ensemble) approaches.  
- CatBoost was the most effective model, delivering strong generalization performance.  
- The model is excellent for identifying *No Revenue* sessions and moderately strong for detecting *Revenue* sessions.  
- Future work can focus on improving precision for the minority class through threshold tuning, cost-sensitive learning, or business-rule integration.  
