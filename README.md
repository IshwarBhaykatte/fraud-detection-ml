# fraud-detection-ml
This project focuses on detecting fraudulent financial transactions using machine learning techniques on a highly imbalanced dataset. The goal is to maximize fraud detection while minimizing missed fraud cases.

# Fraud Detection using Machine Learning

## Overview
This project focuses on detecting fraudulent financial transactions from a highly imbalanced dataset (~0.13% fraud cases).
The goal is to maximize fraud recall while maintaining operational feasibility.

## Problem Statement
In fraud detection, missing a fraudulent transaction is significantly more costly than flagging a legitimate one.
Therefore, recall and ROC-AUC are prioritized over accuracy.

## Approach
- Data cleaning and validation (no missing values)
- Feature engineering using balance changes and log-transformed amounts
- Time-based train-validation split to prevent data leakage
- Model comparison: Logistic Regression, Random Forest, XGBoost

## Final Model
**XGBoost**
- Fraud Recall: ~90%
- ROC-AUC: ~0.997

## Key Features
- Transaction type (TRANSFER, CASH_OUT)
- Origin and destination balance changes
- Transaction amount (log scale)
- Time-based features

## Model Evaluation
![ROC Curve](images/roc_curve.png)
![Confusion Matrix](images/confusion_matrix.png)
![Feature Importance](images/feature_importance.png)

## Tools & Technologies
- Python
- Pandas, NumPy
- Scikit-learn
- XGBoost
- Matplotlib

## Business Impact
The model enables early fraud detection, reduces financial loss, and supports real-time risk-based decision making.

## Future Improvements
- Threshold optimization
- Cost-sensitive learning
- Real-time deployment pipeline
