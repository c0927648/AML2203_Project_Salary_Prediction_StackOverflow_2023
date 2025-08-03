# US Developer Salary Prediction — Stack Overflow 2023

This repository presents a regression-based salary prediction pipeline using the Stack Overflow 2023 Developer Survey. The project focuses exclusively on U.S.-based respondents to model developer compensation using demographic, educational, employment, and technical features. The final solution integrates extensive feature engineering, pruning, and model tuning to improve predictive accuracy and interpretability.

---

## Project Overview

- **Objective**: Predict annual salary (`CompTotal`) for U.S. developers using survey-based features.
- **Dataset**: Stack Overflow Developer Survey 2023 (public release).
- **Sample Size**: 11,111 U.S. respondents after cleaning.
- **Features**: Over 180 processed features, including engineered interactions and domain-specific indicators.

---

## Dataset Source

Stack Overflow Developer Survey 2023  
https://survey.stackoverflow.co/2023/

---

## Data Preparation

- Filtered for U.S.-based respondents with salary reported in USD.
- Removed rows and columns with excessive missing values (50%+ for rows, 70%+ for columns).
- Standardized formats for age, education, company size, and coding experience.
- Converted ordinal and categorical variables using mapping, one-hot, and multi-label encoding.
- Applied log transformation to the `CompTotal` target to correct heavy right-skew.
- Created cleaned and compact binary flags for developer roles, remote work status, and employment type.

---

## Exploratory Data Highlights

- The majority of respondents are aged 18–34, with a dominant presence in the 25–34 bracket.
- Salaries tend to increase with age, peaking in the 35–54 range.
- Developers working remotely or in hybrid setups report higher median salaries than in-person workers.
- Most developers have formal education (bachelor’s or master’s), though alternative paths are also present.
- JavaScript, SQL, Python, and PostgreSQL are among the most used technologies.
- Experience-related fields are highly correlated with each other but show weak linear correlation with salary, requiring non-linear modeling approaches.

---

## Modeling Pipeline

| Stage                          | Description                                                                 |
|-------------------------------|-----------------------------------------------------------------------------|
| Baseline Models               | Trained Random Forest, XGBoost, LightGBM, CatBoost, Gradient Boosting, and Lasso using default parameters |
| Comprehensive Feature Engineering | Created interaction features (e.g., `WorkExp × Role`, `OrgSize × EdLevel`), domain features (`TechBreadth`, `KnowledgeScore`), log-transformed numeric fields, and binned experience levels |
| Feature Pruning               | Dropped features with importance below 0.01 based on CatBoost rankings and retrained all models |
| Hyperparameter Tuning         | Applied `RandomizedSearchCV` to CatBoost, LightGBM, and Gradient Boosting after grid search proved inefficient |
| Final Evaluation              | Compared tuned models using R², RMSE, and MAE on the optimized feature set |

---

## Final Model Comparison

| Model               | R²     | RMSE        | MAE         |
|---------------------|--------|-------------|-------------|
| LightGBM (Tuned)    | 0.4226 | 60,177.42   | 41,901.25   |
| CatBoost (Tuned)    | 0.4200 | 60,435.00   | 42,228.00   |
| Gradient Boosting   | 0.4060 | 61,168.00   | 42,945.00   |

LightGBM showed the strongest overall performance after tuning, closely followed by CatBoost. Gradient Boosting also improved, though the gains were more modest. All models benefited from the engineered feature set and pruning.

---

## Key Findings

- Years of professional coding experience (`YearsCodePro`) consistently ranked as the most important predictor.
- Interaction terms such as `Education × OrgSize` and `WorkExp × Role` significantly boosted model performance.
- Organizational size and educational level both had strong influence on salary, especially when combined.
- Log-transforming the target (`CompTotal`) helped stabilize variance and improve prediction accuracy.
- Remote work status, while widespread, had limited standalone predictive power.
- Features like self-assessed knowledge, tool diversity (`TechBreadth`), and enterprise tech usage added moderate signal.

---

## Future Enhancements

We’ve already seen meaningful improvements from advanced feature engineering, interaction terms, and tuning best models, which helped the models learn non-linear patterns more effectively. To build on this, we plan to expand the dataset beyond 2023 to reduce temporal bias and improve generalization. Incorporating additional context like historical salary trends, cost of living, or job market conditions could further strengthen predictions. We’re also considering shifting from point estimates to predicting salary ranges, which would make the outputs more realistic and useful in applied settings.