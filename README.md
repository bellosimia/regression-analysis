# Marketing ROI Analysis: Simple Linear Regression

## Project Overview

This project analyses a marketing dataset to determine which advertising channel — TV, Radio, or Social Media — has the strongest and most reliable impact on Sales. The analysis uses Python, statsmodels, and OLS (Ordinary Least Squares) regression to build a statistically validated model and translate the results into a clear budget recommendation.

-----

## Environment Setup

### Requirements

- Python 3.8+
- Jupyter Notebook or JupyterLab

### Install dependencies

```bash
pip install pandas numpy statsmodels scipy matplotlib
```

### Launch the notebook

```bash
jupyter notebook regression_analysis.ipynb
```

Run all cells in order (Kernel → Restart & Run All).

-----

## Dataset

**File:** `marketing_and_sales_data_evaluate_lr_csv.csv`

|Column      |Type |Description                           |
|------------|-----|--------------------------------------|
|TV          |int  |TV advertising spend (arbitrary units)|
|Radio       |float|Radio advertising spend               |
|Social_Media|float|Social media advertising spend        |
|Sales       |float|Observed sales revenue                |

- **Raw rows:** 4,572
- **After cleaning:** 4,546 (26 rows dropped due to missing values)
- **Missing values:** TV (10), Radio (4), Social_Media (6), Sales (6)
- **Infinite values:** None

-----

## Analytical Workflow

### Step 1: Load and inspect

Read the CSV, check data types, view descriptive statistics, and understand the range and distribution of each variable.

### Step 2: Handle missing and infinite values

Replace any `inf` or `-inf` values with `NaN`, then drop all rows containing missing data. Document how many rows were removed and confirm the cleaned dataset is complete.

### Step 3: Exploratory Data Analysis

Scatter plots of each channel against Sales reveal the shape of each relationship. A correlation heatmap quantifies the strength of those relationships numerically.

### Step 4: Variable selection

TV has a Pearson correlation of **r = 0.9995** with Sales — by far the strongest of the three channels. Radio (r = 0.87) and Social Media (r = 0.53) are weaker and noisier. TV is selected as the independent variable for the OLS model.

### Step 5: OLS regression with statsmodels

Fit a simple linear regression model using `sm.OLS`:

```
Sales = b0 + b1 * TV
```

The full `model.summary()` output is printed, including coefficients, standard errors, t-statistics, p-values, R-squared, adjusted R-squared, AIC, and BIC.

### Step 6: Assumption diagnostics

Four OLS assumptions are tested:

|Assumption            |Test Used                                          |Result                          |
|----------------------|---------------------------------------------------|--------------------------------|
|Linearity             |Residuals vs Fitted plot                           |Satisfied                       |
|Normality of residuals|Shapiro-Wilk (scipy) + Q-Q plot                    |Satisfied (W = 0.9998, p = 0.91)|
|Homoscedasticity      |Scale-Location plot + Breusch-Pagan (`statsmodels`)|Satisfied                       |
|Independence of errors|Durbin-Watson (`statsmodels`)                      |Satisfied (DW ≈ 2.00)           |

### Step 7: Interpret results

Translate the coefficient, R-squared, p-value, and confidence intervals into plain business language.

### Step 8: Channel comparison

Run separate `sm.OLS` models for each channel. Compare slope and R-squared side by side in bar charts. Confirm findings with a multiple regression using all three channels simultaneously.

### Step 9: Business recommendation

Deliver a clear, evidence-based budget recommendation written for a non-technical stakeholder.

-----

## Key Results

```
OLS Model:  Sales = -0.1325 + 3.5615 * TV

R-squared:          0.9990
Adjusted R-squared: 0.9990
p-value (TV):       < 0.0001
Durbin-Watson:      ~2.00
Shapiro-Wilk:       W = 0.9998, p = 0.91
Breusch-Pagan:      p > 0.05
```

### Channel comparison

|Channel     |Slope|R²   |Reliability|
|------------|-----|-----|-----------|
|TV          |3.56 |0.999|Very high  |
|Radio       |8.36 |0.754|Moderate   |
|Social Media|22.19|0.278|Low        |

Social Media has the highest raw slope but an R² of only 0.28 — meaning its relationship with Sales is inconsistent. When all three channels are entered into a multiple regression simultaneously, Radio and Social Media coefficients collapse to near zero, confirming TV is the dominant driver.

-----

## Business Recommendation

**Allocate the majority of the marketing budget to TV.**

TV is the only channel with both a strong slope and near-perfect consistency (R² = 0.999). Radio is a reasonable secondary channel for reach diversification. Social Media spend should be monitored and analysed further before scaling — its current relationship with Sales is too variable to justify significant investment.

-----

## Repository Structure

```
.
├── regression_analysis.ipynb                     # Main analysis notebook (all cells executed)
├── README.md                                     # This file
└── marketing_and_sales_data_evaluate_lr_csv.csv  # Source dataset
```

-----

## Analytical Reasoning Summary

The central question was: which marketing channel deserves more budget? Raw slope alone is misleading — Social Media had the highest slope per unit spend but the lowest explanatory power. The correct approach evaluates both effect size (slope) and consistency (R²) together. TV dominated on both metrics. Multiple regression confirmed this: once TV is in the model, the other channels add no meaningful predictive value. The OLS model is statistically sound — all four assumptions were verified through formal tests and visual diagnostics — making the TV recommendation fully defensible to a non-technical stakeholder.