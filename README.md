# Car-insurance-intermediate-machine-learning-project
## Project Title
 Car Insurance Claims Analysis

## Objective / Goal

The goal of this project is to predict whether a driver will file an insurance claim based on their demographics, vehicle information, and driving history.

## Dataset Description
- link: https://www.kaggle.com/datasets/sagnik1511/car-insurance-data
  
- Rows represent individual drivers.

- Features include:

AGE, GENDER, RACE, DRIVING_EXPERIENCE, EDUCATION, INCOME, CREDIT_SCORE, VEHICLE_OWNERSHIP, VEHICLE_YEAR, MARRIED, CHILDREN, POSTAL_CODE, ANNUAL_MILEAGE, VEHICLE_TYPE, SPEEDING_VIOLATIONS, DUIS, PAST_ACCIDENTS

Target: OUTCOME (0 = No Claim, 1 = Claim)

## Data Cleaning & Preprocessing

Handle missing values

Convert categorical features (AGE, DRIVING_EXPERIENCE, etc.) to ordinal or one-hot encoding

scale numeric features if needed

## Exploratory Visualizations
1. Driving Experience vs Outcome 

<img width="863" height="553" alt="image" src="https://github.com/user-attachments/assets/f7f05e4e-5ae6-49eb-a11a-d8da0e944eec" />

-  Novice drivers have the highest claims
-  while more experienced drivers (20-29y and 30y+) tend to have fewer claims. This indicates a negative correlation between driving experience and the likelihood of filing a claim.
2. Outcome counts by Age Group
<img width="549" height="465" alt="image" src="https://github.com/user-attachments/assets/77aa238f-1f71-4bd1-b880-6870f4ed595a" />

- Young people shows the highest counts of outcome=1, while middle_aged (40-64)shows the least number of caliming outcome(1)

3.  Speeding Violations vs. OUTCOMEHighest Risk at Zero:
<img width="536" height="397" alt="image" src="https://github.com/user-attachments/assets/5db36131-da19-4884-bd0f-508dc334ae75" />

   
- The proportion of the OUTCOME is highest (nearly 50%) for drivers with zero speeding violations.Risk Drops Sharply: The risk drops significantly after the first violation (from 50% to $\approx 15\%$) and remains relatively low (below 15%) across all other violation counts.
- Confounding Factor: This counter-intuitive pattern suggests that the "0 Violations" category is highly contaminated by a separate, high-risk group (e.g., Novice Drivers), making this variable less reliable as a standalone predictor.

## modeling
- Random forest classification with 83% accurecy


| Model                      | Features           | Accuracy (Test) | Precision (Class 1) | Recall (Class 1) | Notes                                                               |
| -------------------------- | ------------------ | --------------- | ------------------- | ---------------- | ------------------------------------------------------------------- |
| RF Original + SMOTE        | All                | 0.82            | 0.74                | 0.67             | Strong overall accuracy, weaker on claims                           |
| RF + PCA                   | 90% variance       | 0.77            | 0.64                | 0.66             | Dimensionality reduction reduces class 1 performance                |
| RF + Variance Threshold    | Filtered           | 0.82            | 0.74                | 0.68             | Removing low-variance features has little effect                    |
| RF + KMeans Cluster        | +Cluster           | 0.82            | 0.74                | 0.68             | Slight improvement on class 0 precision                             |
| Deep Learning (KerasTuner) | All + tuned layers | 0.73            | 0.55–0.60           | 0.89–0.90        | Excellent at catching Class 1 (high recall), but lower precision and overall accuracy |


## Feature permutation
<img width="862" height="547" alt="image" src="https://github.com/user-attachments/assets/b268f77a-5d0f-465b-8f42-c5a777a9513b" />

Dominant Predictors: The model relies heavily on just three features to make predictions: 
- DRIVING_EXPERIENCE (most important, $\approx 7.8\%$), VEHICLE_YEAR_before 2015 ($\approx 4.2\%$), and VEHICLE_OWNERSHIP ($\approx 3.8\%$).
- Low Impact Features:Variables like SPEEDING_VIOLATIONS and CREDIT_SCORE have negligible importance (close to zero), confirming they are poor standalone predictors of the OUTCOME.
- Conclusion: The analysis confirms that human factor (Experience) and vehicle age are the overwhelming drivers of risk, and the model's accuracy is primarily derived from these two core variables.

  
Recommendations

Goal: Detect insurance claims (class 1) → Deep Learning model preferred for highest recall (0.89)

Goal: General accuracy → Random Forest (original features or variance-filtered)

SMOTE is critical for minority class balancing

Focus on DRIVING_EXPERIENCE and VEHICLE_YEAR in risk analysis

Recommendations for Stakeholders

Claims Prediction Focus:

Use Deep Learning model for detecting potential claims (highest recall)

Prioritize monitoring novice drivers and older vehicles

Risk Assessment & Premiums:

Use Random Forest model if overall accuracy is preferred

Factor in DRIVING_EXPERIENCE and VEHICLE_YEAR in premium calculations

Policy Design & Safety Programs:

Offer training programs for novice drivers

Encourage upgrading older vehicles for lower premiums

Resource Allocation:

Focus claim investigations on high-risk groups identified by the model

Allocate marketing and incentives for safe driving based on predictive features
