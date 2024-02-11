# Temperature Prediction Data Analysis

## 1. Data Preprocessing

1. Select only June, July, August and September
    
    ![1](https://github.com/2024-LGSI-Internship/temp-prediction-colab/assets/42794553/a164c555-a711-490d-9ef5-54e2dc33b261)
    
    - Because May and Oct. have very less data, we are trying to focus on mid-summer.
2. Remove outliers from categorical and continuous variables
    
    ![2](https://github.com/2024-LGSI-Internship/temp-prediction-colab/assets/42794553/1fcebf59-7e58-43c3-805e-c43cf4b5f385)
    
    - For categorical variables, when ‘**A**’ is OFF, ‘**B**’ is F and A, and ‘**C**’ is W
    - For continuous variables, remove outliers using Z-score and Box plot
3. Convert float USER_SET_TEMPERATURE to int values
    
    ![3](https://github.com/2024-LGSI-Internship/temp-prediction-colab/assets/42794553/39f56ef5-3707-4580-8aa6-245f0c5c067f)
    
    - Because we have small amount of float USER_SET_TEMPERATUREs
4. Solve multicollinearity
    
    ![4](https://github.com/2024-LGSI-Internship/temp-prediction-colab/assets/42794553/50285959-5bc0-42c3-9b3d-eb761a974ad3)
    
    - Remove p2 and p10 , remain only p1 because p1-p2-p10 has very high correlation
5. Separate into June/July/August and September to secure the train/validation/test data
    - Use June, July and August data for training and validation
    - Use September for testing
6. Drop less informative columns because these data are not conductive to learn model
    - Drop ‘Date’, ‘A’ and ‘G’ columns
7. One-hot encode the categorical variables to apply categorical data into our model
8. Do Standard Scaling for continuous variables except the temperature data
    - Model performance may be improved by normalizing values having different units
    - ‘L’ is between 0 and 615,605
    - ‘M’ is between 0 and 3,726

## 2. Modeling


1. Try Randomized Search
    
    
    | Model | Parameters | Best Value |
    | --- | --- | --- |
    | Lasso Regression | alpha | 0.059 |
    | Decision Tree | max_depth | 20 |
    |  | max_features | None |
    |  | min_samples_leaf | 15 |
    |  | min_samples_split | 12 |
    | Random Forest | max_depth | 20 |
    |  | max_features | auto |
    |  | min_samples_leaf | 9 |
    |  | min_samples_split | 8 |
    |  | n_estimators | 100 |
    | Gradient Boosting | max_depth | 9 |
    |  | learning_rate | 0.1 |
    |  | min_samples_leaf | 1 |
    |  | min_samples_split | 10 |
    |  | n_estimators | 50 |
    | ADA Boosting | n_estimators | 100 |
    |  | learning_rate | 0.01 |
2. Try Validation
    
    
    | Model | Dataset | MAE | RMSE |
    | --- | --- | --- | --- |
    | LASSO Regression | Valid | 1.07 | 1.49 |
    |  | K-Fold Cross Validation | 1.07 | 1.48 |
    | Decision Tree | Valid | 0.97 | 1.41 |
    |  | K-Fold Cross Validation | 0.98 | 1.42 |
    | Random Forest | Valid | 0.90 | 1.32 |
    |  | K-Fold Cross Validation | 0.90 | 1.32 |
    | Gradient Boosting | Valid | 0.91 | 1.32 |
    |  | K-Fold Cross Validation | 0.90 | 1.31 |
    | ADA Boosting | Valid | 1.18 | 1.61 |
    |  | K-Fold Cross Validation | 1.17 | 1.60 |
3. Final Metrics
    
    
    | Model | MAE | RMSE | Model Size | Model Latency | E2E Latency |
    | --- | --- | --- | --- | --- | --- |
    | LASSO Regression | 1.15 | 1.59 | 0.64KB | 0.63ms | 11ms |
    | Decision Tree | 1.15 | 1.63 | 457KB | 1.05ms | 12ms |
    | Random Forest | 1.05 | 1.50 | 48,270KB | 9.41ms | 19ms |
    | Gradient Boosting | 1.08 | 1.54 | 919KB | 0.90ms | 12ms |
    | ADA Boosting | 1.27 | 1.78 | 31KB | 13.79ms | 23ms |
