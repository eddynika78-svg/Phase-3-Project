# Phase-3-Project
# Telecom Customer Churn Prediction – Phase 3 ProjecT
# Overview

This project addresses the critical business problem of **customer churn** in the telecommunications industry. Churn occurs when customers stop using a telecom service provider, leading to significant revenue loss and increased customer acquisition costs.

The goal of this project is to **build a predictive classification model** that identifies customers at high risk of churning, enabling the company to implement targeted retention strategies.

**Main business questions explored:**
- How do additional services (international plan, voice mail plan) influence churn?
- Which geographic regions show the highest churn rates?
- At what point do customer service calls become a strong predictor of churn?

**Dataset used:**  
The well-known **SyriaTel Customer Churn dataset** (commonly referred to as the "Telecom churn dataset"), containing 3,333 customer records with 20 features including usage patterns, plan subscriptions, geographic information, customer service interactions, and the binary target variable `churn` (True = churned, False = stayed).

**Chosen stakeholder:**  
**Customer Retention / Marketing Manager** at a mid-sized telecommunications company

## Business and Data Understanding

### Business Problem
Customer acquisition is significantly more expensive than retention. Reducing churn by even 5–10% can deliver substantial financial impact.  
The company wants to:
- Identify at-risk customers **before** they leave
- Understand the main drivers of churn
- Design cost-effective retention interventions (discounts, bundles, proactive support)

### Why this dataset was chosen
- Classic, widely-used telecom churn dataset → excellent for learning classification
- Contains realistic features: usage metrics, plan types, geography, service interactions
- Binary target variable → perfect fit for classification modeling
- Clean structure with no missing values in most versions
- Allows meaningful business questions to be asked and answered

### Key initial findings (from EDA)
- Overall churn rate: ~14.5%
- International plan users churn at ~42% (vs ~11% without)
- Voice mail plan users churn at ~9% (strong retention signal)
- Churn spikes dramatically at **4+ customer service calls** (~46–60%)
- Highest churn states: CA, NJ, TX, MD (~25–26%)

These insights guided feature selection and modeling decisions.

## Modeling

### Approach
We followed an iterative modeling process:

1. **Baseline model**  
   Logistic Regression (interpretable linear model) with balanced class weights

2. **Non-parametric model**  
   Single Decision Tree (max_depth=5) – interpretable, captures non-linear patterns

3. **Ensemble model**  
   Random Forest (200 trees, tuned parameters) – final chosen model due to best performance and robustness

### Preprocessing pipeline (scikit-learn)
- Numerical features: median imputation + StandardScaler
- Categorical features: constant imputation + OneHotEncoding
- Stratified train-test split (80/20)
- No data leakage (fit transformers only on training data)

### Final model: Random Forest
- Chosen because:
  - Highest ROC-AUC (~0.89–0.91 on test set)
  - Best recall for churn class (~0.79–0.85)
  - Handles feature interactions naturally (e.g., high charges + international plan)
  - More robust to noise than single tree

## Evaluation

### Chosen metrics (business-aligned)
- **ROC-AUC** → overall discrimination ability (main ranking metric)
- **Recall (for churn class = 1)** → most important: we want to catch as many potential churners as possible
- **Precision (churn class)** → avoid wasting retention budget on false positives
- **Accuracy** → reported for completeness

### Final performance (test set)

| Model              | ROC-AUC | Churn Recall | Churn Precision | Accuracy |
|---------------------|---------|--------------|------------------|----------|
| Random Forest       | 0.908   | 0.793        | 0.712            | 0.946    |
| Decision Tree       | 0.875   | 0.784        | 0.645            | 0.928    |
| Logistic Regression | 0.861   | 0.691        | 0.678            | 0.937    |

**Conclusion**: Random Forest gives the best balance — catches ~79% of actual churners while maintaining reasonable precision.

## Conclusion

This project demonstrates how classification modeling can help a telecom company **proactively identify and retain at-risk customers**.

Key actionable insights:
- Voice mail plan is a strong retention driver → promote bundles
- International plan + high usage = very high risk → targeted offers needed
- 4+ service calls = critical warning sign → trigger immediate outreach
- Geographic hotspots (CA, NJ, TX, etc.) deserve focused campaigns

**Business impact estimate**:  
If the model identifies ~80% of churners early and interventions retain 20–30% of them → potential reduction of churn by 3–5 percentage points → millions in saved revenue annually.

**Next steps**:
- Deploy model in production (monthly scoring)
- Run A/B tests on recommended interventions
- Monitor model performance over time (concept drift)
- Integrate with CRM for automated alerts
