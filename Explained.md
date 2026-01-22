
# TASK ANSWER

## __Q.1. Data Cleaning (Missing Values, Outliers, Multicollinearity)__

What I did

__Missing values:__

    > The dataset contained no missing values, which was verified explicitly using column-wise null checks. Therefore, no imputation was required.

__Outliers:__

    > Transaction amounts were highly skewed with extreme values.
  
    > Instead of removing outliers (which would risk deleting real fraud cases), a log transformation was applied to the transaction amount to stabilize variance and reduce the impact of extreme values.

__Multicollinearity:__

    > Original balance features (oldbalanceOrg, newbalanceOrig, oldbalanceDest, newbalanceDest) were highly correlated.

To address this, derived features were created:
- orig_balance_change
- dest_balance_change
This reduced redundancy and improved model interpretability.



## __Q.2. Fraud Detection Model (Detailed Explanation)__

Model used
XGBoost (Extreme Gradient Boosting)

__Why XGBoost__-

  * Handles non-linear relationships better than Logistic Regression.
  
  * Performs well on imbalanced datasets.
  
  * Supports class weighting (scale_pos_weight).
  
  * Robust to noisy and correlated features.

 **Model objective**-
 
   - Binary classification (fraud vs non-fraud).
     
   - Primary goal: maximize fraud recall, not accuracy.

__Key configuration__-

    objective = binary:logistic
     
    eval_metric = auc
     
    scale_pos_weight tuned based on class imbalance
     
    Moderate tree depth to avoid overfitting



## __Q.3. Variable Selection Strategy__

How features were selected

  1. __Domain logic__
  
     - Fraud often involves abnormal balance movements.
     
     - Certain transaction types (TRANSFER, CASH_OUT) are known fraud vectors.

  3. __Feature engineering__

     - Balance change features replaced raw balances.
     
     - Time-based feature (hour) extracted from transaction step.

  4. __Model-driven selection__

     - Feature importance from tree-based models.
    
     - Only features contributing predictive value were retained.

Final feature set

- type
- log_amount
- orig_balance_change
- dest_balance_change
- is_transfer
- is_cashout
- hour



## __Q.4. Model Performance Evaluation__
 
__Metrics used (IMPORTANT)__

   - Recall (Fraud class) → **business-critical**
   
   - Precision (Fraud class) → **operational cost**
   
   - ROC-AUC → **ranking ability across thresholds**
   
   - Confusion Matrix

__Why not accuracy?__

  Due to extreme class imbalance (~0.13% fraud rate), accuracy is a misleading metric and does not reflect the model’s effectiveness in detecting fraudulent transactions.

__Final XGBoost performance__

    Fraud Recall: ~70%

    Fraud Precision: ~9%

    ROC-AUC: ~0.851

This is realistic and defensible for a fraud system.


## __Q.5. Key Factors Predicting Fraud__

__Top predictors:-__

    Origin balance change.
   
    Destination balance change.
   
    Transaction type (TRANSFER, CASH_OUT).
   
    Transaction amount (log-scaled).
   
    Transaction timing (hour).

These features directly capture money movement inconsistencies.


## __Q.6. Do These Factors Make Sense?__

**Yes — completely**.

- Why?
 
  ->Fraudsters attempt to empty accounts → abnormal balance drops.
  
  ->Destination accounts often show inconsistent balance updates.
  
  ->TRANSFER and CASH_OUT bypass merchant verification.
  
  ->Large or unusual amounts raise risk.
  
  ->Fraud often clusters during specific time windows.

Nothing here is random or accidental.

_What would NOT make sense_?
- If customer names or IDs were top features → that would indicate data leakage.
  Your model does not suffer from this.


## __Q.7. Prevention Measures for Infrastructure Update__
   
__Recommended actions:-__
 
    Real-time monitoring for abnormal balance changes.
    
    Rule-based pre-filters before ML scoring.
    
    Adaptive thresholds based on transaction type.
    
    Step-up authentication for high-risk transactions.
    
    Continuous model retraining with new fraud patterns.

Important note:-
ML does not replace rules — it augments them.


## __Q.8. How to Measure if Prevention Works__

__KPIs to track:-__
 
    Fraud loss rate (₹ lost / total transaction value).
    
    Fraud recall over time.
    
    False positive rate.
    
    Manual review load.
    
    Customer complaint rate.

__Method:-__

    A/B testing (new system vs old).
    
    Monitor concept drift.
    
    Regular backtesting on recent data.

If recall stays high without exploding false positives, the system works.


# FINDED WHY ROC DECREASE ?
   After cleaning duplicate pipelines, ROC dropped because earlier runs unintentionally mixed sampling and feature paths, causing optimistic validation.
   
   The final model uses a single, leakage-free pipeline, giving a more realistic ROC around 0.86, which better reflects real-world deployment.


# ___________THANK YOU AND HAVE A GOOD DAY___________