# Insurance Cost Prediction Project

## Project Description

This project aims to analyze and preprocess a dataset containing information about individuals and their insurance costs. The goal is to understand the factors influencing insurance charges and prepare the data for potential modeling.

## Data Source

The data used in this project is sourced from `insurance.csv`.

## Data Exploration

Initial data exploration was performed to understand the dataset's structure, size, and basic statistics.

- The dataset contains 1338 rows and 7 columns.
- The columns include: `age`, `sex`, `bmi`, `children`, `smoker`, `region`, and `charges`.
- Data types of the columns were inspected, revealing a mix of numerical and object types.
- Descriptive statistics for numerical columns (`age`, `bmi`, `children`, `charges`) were generated.
- Checked for missing values, and none were found.
- Inspected the unique values and counts for categorical columns (`sex`, `smoker`, `region`).

Visualizations were used to explore the distribution of numerical features and the counts of categorical features:

- Histograms and KDE plots for `age`, `bmi`, `children`, and `charges` showed the distributions of these features.
- Count plots for `sex`, `children`, and `smoker` displayed the frequency of each category.
- Box plots for numerical columns were generated to visualize potential outliers.
- A heatmap of the correlation matrix for numerical features was created, showing the relationships between `age`, `bmi`, `children`, and `charges`.

## Data Cleaning and Preprocessing

The following steps were taken to clean and preprocess the data:

- A copy of the original DataFrame (`df_cleaned`) was created to avoid modifying the original data.
- Duplicate rows were identified and removed, resulting in 1337 unique entries.
- Checked for null values again after dropping duplicates to ensure data integrity.
- Data types were confirmed before further processing.
- The `sex` column was mapped to numerical values (male: 0, female: 1).
- The `smoker` column was mapped to numerical values (no: 0, yes: 1).
- Columns were renamed for clarity (`sex` to `is_female`, `smoker` to `is_smoker`).
- One-hot encoding was applied to the `region` column to convert categorical regions into numerical features, dropping the first category to avoid multicollinearity.
- The resulting one-hot encoded columns were converted to integer type.

## Feature Engineering and Extraction

Feature engineering was performed by creating a new categorical feature based on 'bmi':

- The `bmi` column was categorized into 'Underweight', 'Normal', 'Overweight', and 'Obese' using `pd.cut`.
- One-hot encoding was applied to the `bmi_category` column, dropping the first category.
- The resulting one-hot encoded columns were converted to integer type.

Numerical features (`age`, `bmi`, `children`) were standardized using `StandardScaler` to have a mean of 0 and a standard deviation of 1.

Feature selection was performed using Pearson correlation for numerical features and Chi-Squared test for categorical features against the target variable `charges`:

- **Pearson Correlation:** Calculated the Pearson correlation coefficient between selected numerical and one-hot encoded features and `charges`. The results were sorted to identify features with stronger linear relationships. 'is_smoker' showed the highest positive correlation with 'charges'.
- **Chi-Squared Test:** Performed Chi-Squared tests for independence between selected categorical features and a binned version of the `charges` column. Features with a p-value less than 0.05 were considered significant and kept. 'is_smoker', 'region_southeast', 'is_female', and 'bmi_category_Obese' were found to be statistically significant.

Based on these analyses, a `final_df` was created containing the features deemed most relevant for predicting insurance charges: `age`, `is_female`, `bmi`, `children`, `is_smoker`, `charges`, `region_southeast`, and `bmi_category_Obese`.
