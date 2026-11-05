# Cost of Living and Hotel Bookings Analysis (R)

An R analysis of global cost-of-living data and hotel reservation data, covering city clustering, room price regression, and booking cancellation classification.

Individual coursework project for ST3189 Machine Learning, SIM-UOL, 2026.

## Table of Contents

* [Overview](#overview)
* [Research Questions](#research-questions)
* [Data](#data)
* [Methods](#methods)
* [Key Results](#key-results)
* [Repository Structure](#repository-structure)
* [Running the Code](#running-the-code)
* [Author](#author)
* [License](#license)

## Overview

The full write-up is in [`ml_report.pdf`](ml_report.pdf) and the analysis code is in [`ml.Rmd`](ml.Rmd).

| Task | Dataset | Goal |
|------|---------|------|
| Unsupervised learning | Global Cost of Living (Numbeo) | Group cities with similar living costs and find the main dimensions of variation |
| Regression | Hotel Reservations | Predict average hotel room price per night |
| Classification | Hotel Reservations | Predict whether a booking will be cancelled |

## Research Questions

**Unsupervised learning**
1. What is the optimal number of clusters to group cities?
2. What are the key factors differentiating the cost-of-living clusters?
3. What are the principal dimensions of variation in global living costs?

**Regression**
1. What factors influence average hotel room prices per night?
2. Which regression model predicts room prices best?
3. How does regularisation affect model performance and feature selection?

**Classification**
1. What is the best model for predicting booking cancellations?
2. Which features best identify cancellation-risk reservations?
3. How do the different classification models compare?

## Data

Copies of the data are included in this repository (`cost-of-living.csv` and `Hotel Reservations.csv`). The original sources are linked below.

| Dataset | Size | Used for | Source |
|---------|------|----------|--------|
| Global Cost of Living | ~4,700 cities, 58 columns (55 price variables) | Unsupervised learning | [Kaggle](https://www.kaggle.com/datasets/mvieira101/global-cost-of-living/data) |
| Hotel Reservations | 36,275 rows, 19 columns | Regression and classification | [Kaggle](https://www.kaggle.com/datasets/ahsan81/hotel-reservations-classification-dataset) |

**Preprocessing**
- *Cost of living:* variables with more than 30% missing values were dropped (`x28`, `x29`, `x40`, `x43`, `x51`, `x52`, `x53`), remaining gaps were filled with the median, and prices were standardised before PCA.
- *Hotels:* `booking_status` was removed for the regression task to prevent data leakage. Both tasks use an 80:20 train-test split.

## Methods

| Task | Techniques | Evaluation |
|------|-----------|------------|
| Unsupervised | PCA, K-means, hierarchical clustering | Scree plot, elbow method, silhouette score, dendrogram |
| Regression | Linear regression, Ridge, Lasso, Random Forest | RMSE, MAE, R² on the test set; 10-fold CV |
| Classification | Logistic regression, KNN, SVM, Random Forest | Accuracy, sensitivity, specificity, F1, AUC; cross-validation |

## Key Results

### Unsupervised learning
- **Two clusters** were identified by the elbow method, silhouette score, and dendrogram, broadly splitting cities into higher-cost and lower-cost groups.
- The median monthly net salary was about USD 2,284 in the higher-cost cluster versus USD 777 in the lower-cost cluster.
- The first principal component explains ~37% of variance (dining out, leisure, and beverage prices); PC2 explains ~7% (everyday costs such as energy, clothing, and market food). The first 19 PCs explain over 80% of variance.

### Regression (average room price, EUR)

| Model | RMSE | MAE | R² |
|-------|------|-----|----|
| Ridge | 22.97 | 17.57 | 0.508 |
| Lasso | 22.95 | 17.54 | 0.508 |
| **Random Forest** | **14.14** | **8.91** | **0.818** |

Regularisation barely improved on the linear model, and Lasso kept all 26 features. Random Forest captured non-linear effects, notably seasonality through arrival month. Room type, arrival month, and number of guests were the main drivers of price.

### Classification (booking cancellation)

| Model | Accuracy (%) | Sensitivity (%) | Specificity (%) | F1 (%) | AUC |
|-------|-------------|-----------------|-----------------|--------|-----|
| Logistic Regression | 80.73 | 64.07 | 88.85 | 68.54 | 0.870 |
| KNN (k = 9) | 80.43 | 58.60 | 91.06 | 66.24 | 0.851 |
| SVM | 83.83 | 68.74 | 91.18 | 73.59 | 0.896 |
| **Random Forest** | **90.34** | **81.70** | **94.55** | **84.71** | **0.959** |

Random Forest performed best on every metric. **Lead time** was the most important predictor of cancellation, followed by average room price and number of special requests.

## Repository Structure

```
.
├── README.md
├── ml.Rmd                  # Full analysis: unsupervised, regression, classification
├── ml_report.pdf           # Written report
├── cost-of-living.csv      # Global Cost of Living dataset
└── Hotel Reservations.csv  # Hotel Reservations dataset
```

## Running the Code

1. Clone or download the repository so the two CSV files sit alongside `ml.Rmd`.
2. Open `ml.Rmd` in RStudio (R 4.x) and install any packages listed in the setup chunk.
3. Knit the file, or run the chunks from top to bottom.

## Author

Liu Yu Xun · https://www.linkedin.com/in/yu-xun-liu/

This was completed as an individual project.

## License

The analysis code and written report are © Liu Yu Xun 2026. All rights reserved.
The datasets (`cost-of-living.csv` and `Hotel Reservations.csv`) belong to their
original providers and are not covered by this notice.
