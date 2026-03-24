# Customer Lifetime Value Analysis for Oberalp Sales Data

An end-to-end customer analytics project that combines **e-commerce** and **retail** transaction data to estimate and predict **Customer Lifetime Value (CLV)** for the **Salewa** brand under the Oberalp Group.

This project was developed as a capstone project at the **Free University of Bolzano** by **Abhishek Bargujar** , under the supervision of **Floriano Zini**.

---

## Table of Contents

- [Project Overview](#project-overview)
- [STAR Summary](#star-summary)
  - [Situation](#situation)
  - [Task](#task)
  - [Action](#action)
  - [Result](#result)
- [Business Problem](#business-problem)
- [Dataset Description](#dataset-description)
- [Data Preparation](#data-preparation)
- [Methodology](#methodology)
  - [1. Formula-Based CLV](#1-formula-based-clv)
  - [2. Feature Engineering](#2-feature-engineering)
  - [3. Machine Learning Models](#3-machine-learning-models)
  - [4. Evaluation Metrics](#4-evaluation-metrics)
- [Key Findings](#key-findings)
- [Project Architecture](#project-architecture)
- [Technologies Used](#technologies-used)
- [Challenges](#challenges)
- [Future Improvements](#future-improvements)
- [Conclusion](#conclusion)

---

## Project Overview

Customer Lifetime Value (CLV) is one of the most important metrics in business analytics because it estimates the total value a customer is expected to generate during their relationship with a company. In practice, CLV helps businesses make better decisions about:

- customer retention
- marketing investment
- revenue forecasting
- customer segmentation
- long-term growth strategy

In this project, we analyzed transaction data from **two different sales channels**:

1. **E-commerce transactions**
2. **Retail store transactions**

The main goal was to create a reliable pipeline for:

- cleaning and standardizing both datasets
- isolating relevant **Salewa** transactions
- merging both channels into one unified customer view
- computing CLV with a formula-based baseline
- predicting CLV with machine learning models
- comparing model-based and formula-based approaches
- extracting actionable insights about customer behavior and future value

---

# STAR Summary

## Situation

Many companies rely on simple heuristics or static formulas to estimate customer value. These methods are often easy to apply, but they usually fail to capture the real complexity of customer behavior.

In this project, the challenge was especially difficult because the data came from **two different sources** with different structures, date ranges, formats, and customer behavior patterns:

- **E-commerce data** only contained Salewa online transactions
- **Retail data** included multiple brands and had to be filtered to keep only Salewa-related sales

This created several practical problems:

- inconsistent schemas across data sources
- different date and time formats
- missing or incompatible fields
- the need to identify the same customer across channels
- skewed revenue distributions and extreme outliers
- changing customer behavior from one year to another

Without a proper integration and modeling pipeline, CLV estimation would remain noisy, inconsistent, and less useful for business decision-making.

---

## Task

The objective of this project was to build a complete analytical workflow that could answer the following question:

> **How can we accurately estimate and predict customer lifetime value using historical transaction data from both retail and e-commerce channels?**

More specifically, the project required us to:

- standardize two heterogeneous datasets
- filter and retain only **Salewa** transactions
- concatenate the data in a meaningful and analysis-ready format
- derive customer-level CLV variables such as:
  - frequency
  - recency
  - tenure
  - monetary value
- calculate CLV using a baseline formula
- design feature engineering steps to improve predictive power
- train and compare machine learning models
- evaluate model performance using multiple metrics
- forecast future CLV and interpret business implications

The broader goal was not only to build a model, but to understand **which approach is most useful and realistic for business use**.

---

## Action

To solve the problem, the project was structured into several stages.

### 1) Data understanding and source alignment

We first studied both datasets carefully to understand their structure, granularity, and relevance.

#### E-commerce dataset
This dataset contained Salewa online transactions and included fields such as:

- transaction ID
- order number
- order time
- customer number
- Oberalp ID
- product/article number
- price
- quantity
- return-related fields

#### Retail dataset
This dataset included in-store transactions across multiple brands, so it required filtering before use. Relevant fields included:

- date and time
- product code / article code
- total price including tax
- quantity
- Oberalp customer ID

Because the project focuses on **Salewa**, only product codes representing Salewa transactions were retained.

---

### 2) Data cleaning and preprocessing

The next step was to standardize and clean the data so both sources could be merged.

Key preprocessing steps included:

- converting date and time fields into consistent formats
- separating date and time where needed for cleaner analysis
- filtering product codes to retain only Salewa items
- removing very small numbers of null or incompatible records
- excluding returns and non-positive retail revenue records
- ensuring customer identifiers were usable for omnichannel analysis
- creating a common **revenue** variable for each transaction

Revenue was derived as:

- **E-commerce:** `price × quantity`
- **Retail:** `total tax included × quantity`

This step was critical because monetary value is one of the core CLV components.

---

### 3) Dataset integration

After cleaning, both channels were aligned on selected common columns and concatenated into one unified transaction-level dataset.

This allowed us to move from isolated channel-level analysis to a broader **customer-centric view**, where each customer’s value could be studied across time and channels.

This integration was essential because:

- a single customer may interact through more than one sales channel
- customer value is underestimated if channels remain separate
- omnichannel behavior provides stronger signals for CLV prediction

---

### 4) Baseline CLV calculation

Before using machine learning, we implemented a formula-based CLV baseline.

The formula used in the project was:

```text
CLV = Monetary Value × Frequency / (Frequency + 1)
```

This formula served as:
- a simple interpretable benchmark
- a business-friendly baseline
- a reference point for comparing ML predictions

To support this stage, transaction data was transformed into customer-level metrics using the lifetimes ecosystem and CLV-related variables such as:
- frequency
- recency
- tenure
- monetary value



### 5) Exploratory analysis and behavioral understanding
- Before moving to predictive models, we explored customer behavior patterns in the data.

- This included examining:
  - repeat purchase frequency
  - actual vs expected future purchases
  - probability alive style interpretations
  - future purchase intensity patterns
  - cohort behavior of top CLV customers
  - 2023 vs 2024 monthly revenue patterns
  - variation and outliers in actual vs predicted CLV distributions

- This stage helped us understand that customer behavior was highly non-linear and that some customers showed very strong differences across years, making simple static models less reliable.



### 6) Feature engineering

- Raw transactional data is usually not strong enough on its own for ML-based CLV prediction, so we engineered additional features to help the models learn customer patterns more effectively.

- Engineered features included:

1. customer lifespan
- Number of days between first and most recent purchase
2. average time between purchases
- Helps distinguish frequent buyers from sporadic ones
3. purchase frequency ratio
- Frequency relative to customer tenure
4. log transformation of monetary values
- Used to reduce skewness and control the effect of extreme high-value customers

- We also applied scaling and normalization techniques such as:
  - standard scaling
  - quantile transformation toward a normal-like distribution

- These steps helped stabilize the feature space and reduce bias from very large numerical values.



### 7) Model development

- We tested multiple approaches for CLV prediction.

a) Formula-based approach
- A simple benchmark using the business-oriented CLV formula.

b) BG/NBD experimentation
- A probabilistic customer-base model was explored to estimate future purchasing patterns. It was useful from an experimental perspective, but it did not perform strongly enough for the final predictive goal.

c) Machine learning models
- We trained and compared:
   - XGBoost (XGB)
   - Extra Trees Regressor (ETR)
   - Multi-Layer Perceptron (MLP)

- These were chosen because CLV behavior is often non-linear, noisy, and affected by outliers.

- Why XGBoost?
 XGBoost is strong at capturing non-linear relationships, handling complex interactions, and ranking the importance of variables.

- Why Extra Trees Regressor?
 ETR introduces randomness through ensembles of decision trees, helping reduce overfitting and generalize better.

- Why MLP?
 MLP was tested because neural networks can capture hidden patterns in customer behavior, especially when relationships are not simple or linear.


### 8) Evaluation design

- We compared the models using standard regression metrics:
  - MAE – Mean Absolute Error
  - RMSE – Root Mean Squared Error
  - R² – explained variance

The analysis included:
- actual vs predicted CLV for 2024
- formula-based vs model-based CLV for 2025
- top 10 customer comparisons
- distribution and outlier analysis through box plots
- customer-level interpretation through monthly cohort tables

- We also studied how model quality changed when the forecasting horizon increased.



## Result

The project showed that machine learning clearly outperformed traditional formula-only approaches for CLV estimation.

## Main results
XGBoost achieved the best performance
It explained most of the variance in CLV values and gave the closest estimates to actual customer value
ETR performed reasonably well, but below XGBoost
MLP was the weakest among the three main ML models, likely because of data complexity and outlier sensitivity
BG/NBD did not generalize well enough for this use case


## 2024 prediction performance

For the 2024 setting, XGBoost achieved very strong performance and reached the highest R² score, showing that it was able to capture the structure of customer value very effectively.

## 2025 forecast performance

When forecasting further into the future, performance dropped. This is expected in CLV problems because:

customer behavior changes over time
longer forecasting horizons introduce uncertainty
historical patterns do not always repeat cleanly

## In the 2025 setup:

the formula-based method predicted slightly higher CLV values on average
the model-based approach was more conservative
some customers who appeared in the top CLV group in 2024 did not remain there in the 2025 forecast

This indicates a meaningful shift in customer purchase behavior.

## Business insights

The project also revealed that:

customer value is highly unevenly distributed
a small set of customers contributes disproportionately high CLV
outliers matter a lot
loyalty-linked or identifiable customers can provide stronger business value signals
shorter and more recent observation windows often produce more reliable predictions than longer-range forecasts
What this project achieved

## In practical terms, this project delivered:

an integrated multi-source CLV pipeline
a formula-based benchmark for interpretability
a machine learning framework for improved prediction
clear evidence that tree-based models outperform simpler baselines
customer-level and cohort-level insights that can support:
retention strategy
customer prioritization
targeted marketing
revenue planning




## Business Problem

The business problem behind this project is simple but very important:

Not all customers generate the same long-term value, and businesses need reliable ways to identify which customers matter most.

A poor CLV estimation system leads to:

inefficient marketing spend
poor targeting
underestimation of high-value customers
weak retention strategy
inaccurate revenue expectations

By improving CLV estimation, businesses can allocate resources more intelligently and make more confident decisions.


## Dataset Description

The project uses two main sources of transactional data.

E-commerce data

Contains Salewa online transactions.

Typical fields include:

transaction ID
order number
order time
customer number
Oberalp ID
article order number
price
quantity
Retail data

Contains store transactions and originally includes multiple brands.

## Typical fields include:

date
time
product/article code
total tax included
quantity
Oberalp ID
Important filtering logic

Only transactions representing Salewa products were retained in the analysis.

## Data Preparation

The data preparation phase included:

schema harmonization
date/time standardization
null handling
filtering of negative and zero-value sales where appropriate
removal of irrelevant transactions
revenue derivation
selection of compatible columns for concatenation

This stage was one of the most important parts of the project because model quality depends heavily on data Quality.



## Methodology

### 1. Formula-Based CLV

Baseline formula:

CLV = Monetary Value × Frequency / (Frequency + 1)

This formula was used because it provides a clear, interpretable benchmark for CLV estimation.


### 2. Feature Engineering

Created features included:

customer lifespan
average time between purchases
purchase frequency ratio
log-transformed monetary value

Additional scaling and transformation steps helped improve learning behavior and reduce the impact of skewed distributions.




### 3. Machine Learning Models

XGBoost

Best-performing model overall. Strong at learning complex non-linear relationships.

Extra Trees Regressor

Stable ensemble model with decent robustness and lower overfitting risk.

Multi-Layer Perceptron

Useful for testing deeper pattern learning, but less effective here due to data complexity and outliers.

BG/NBD

Explored as a probabilistic approach, but not strong enough in final predictive performance.




### 4. Evaluation Metrics

The following metrics were used:

MAE
RMSE
R²

These provided a balanced evaluation of prediction error, robustness, and explained variance.


### Key Findings
Machine learning outperformed formula-only CLV estimation
XGBoost was the strongest model
ETR was acceptable but weaker
MLP underperformed relative to tree-based methods
BG/NBD did not generalize well enough for this task
Predicting nearer time horizons was more accurate than long-term forecasts
Outliers strongly influenced CLV estimation
Some high-value customers changed behavior significantly from one year to another
Customer segmentation and loyalty-linked features appear highly valuable for CLV work


### Project Architecture

The logical flow of the project can be summarized as follows:

Raw E-commerce Data
        \
         \
          --> Data Cleaning & Standardization --> Data Integration --> Customer-Level Aggregation
         /
        /
Raw Retail Data

Customer-Level Aggregation
        |
        +--> Formula-Based CLV Calculation
        |
        +--> Feature Engineering
                |
                +--> Model Training (XGBoost / ETR / MLP)
                |
                +--> Evaluation (MAE / RMSE / R²)
                |
                +--> Forecasting & Customer Insights



### Technologies Used
Python
Pandas
NumPy
scikit-learn
XGBoost
lifetimes
Matplotlib / Seaborn
Jupyter Notebook


### Challenges

This project involved several real-world analytical challenges.

1. Multi-source inconsistency

The two datasets had different formats, structures, and scopes.

2. Brand filtering

Retail data included multiple brands, so careful filtering was required to isolate Salewa transactions.

3. Outliers

A few customers had very high CLV values, which made learning and evaluation harder.

4. Temporal instability

Customers did not behave consistently across years.

5. Forecasting uncertainty

As the prediction horizon extended toward 2025, model accuracy decreased.

6. CLV complexity

CLV is inherently non-linear and difficult to model with simple assumptions.


### Future Improvements

Several extensions could make the system stronger in future work:

add customer segmentation before modeling
combine probabilistic CLV methods with ML models
test time-series models such as:
ARIMA
Prophet
test deep learning models such as:
LSTM
Transformer-based approaches
incorporate more behavioral and engagement features
improve handling of extreme-value customers
enrich the data with additional CRM or loyalty attributes


### Conclusion

This project demonstrates that CLV prediction becomes much more effective when traditional business formulas are combined with strong data preparation, feature engineering, and machine learning.


- The work shows that:

integrated retail + e-commerce analysis is valuable
good preprocessing is essential
machine learning provides better predictive power than simple formulas alone
XGBoost is especially effective for non-linear CLV estimation
long-term CLV forecasting remains challenging and should be approached carefully

Overall, this project provides both a strong analytical pipeline and useful business insight into how customer value can be measured, predicted, and acted upon more effectively.
