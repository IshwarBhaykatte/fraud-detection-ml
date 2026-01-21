
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

### ROC Curve

<img width="1920" height="1080" alt="ROC-curve" src="https://github.com/user-attachments/assets/65579e39-7301-48bd-aac1-014bcb486abd" />

### Confusion Matrix

<img width="1920" height="1080" alt="Confusion Matrix)" src="https://github.com/user-attachments/assets/843a01e6-69e2-4dd2-932c-961bcdfc53f8" />


### Feature Importance (XGBoost)

<img width="1920" height="1080" alt="Feature Importance Plot (XGBoost)" src="https://github.com/user-attachments/assets/d3576455-9e4d-4a3e-a547-41cf9b1eafb0" />



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
