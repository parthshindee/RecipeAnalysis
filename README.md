# Predicting Recipe Ratings

**Author:** Parth Shinde

**Project Overview:**
This project aims to predict the average rating of recipes on a popular cooking platform using various recipe features such as preparation time, number of ingredients, nutritional content, and user-provided tags. By developing a predictive model, this project seeks to enhance recipe recommendations and assist recipe creators in optimizing their content to meet user preferences.

### 1. Introduction
This section introduces the dataset and the main question driving the project: "Can we predict the average rating of a recipe based on its features?" It explains the significance of this question and the potential applications of a successful predictive model.

**Dataset Details:**
- The dataset contains recipes from a popular cooking platform, including information such as preparation time, ingredients, user ratings, and nutritional values.
- Relevant columns: `minutes`, `n_ingredients`, `tags`, `nutrition`, `description`, `rating`
- Description of relevant columns: Includes preparation time, number of ingredients, recipe tags, nutritional information, user-provided description, and average ratings.

### 2. Data Cleaning and Exploratory Data Analysis
This section details the steps taken to clean the data, including handling missing values, parsing string lists, and creating new features. It also showcases univariate and bivariate analyses with Plotly plots to explore the distributions of key variables and their relationships.

**Data Cleaning Steps:**
- Replaced invalid data with NaN values.
- Extracted numerical data from strings and lists.
- Created new features such as `calories` and `description_length`.

**Exploratory Analysis:**

#### Univariate Analysis

We explored the distribution of key variables to understand the overall patterns in the data.

**Distribution of Average Ratings:**
<iframe
  src="assets/distribution_of_average_ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot above shows that the distribution of average ratings is skewed towards higher values, with most ratings clustered around 4.5, indicating a generally positive reception of recipes.

**Distribution of Calories:**
<iframe
  src="assets/distribution_of_calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Calories are heavily right-skewed, indicating the presence of recipes with exceptionally high calorie counts, pulling the mean above the median.

#### Bivariate Analysis

To explore potential relationships between variables, we examined how certain features interact with each other.

**Top 10 Tags with the Highest Average Ratings:**
<iframe
  src="assets/top_tags_with_highest_ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This bar plot displays the top 10 tags associated with the highest average ratings. It highlights which types of recipes are most favorably reviewed, providing insights into user preferences based on recipe categorization.

### 3. Assessment of Missingness
This section examines the nature of missing data in the dataset, exploring whether certain missing values are NMAR (Not Missing At Random) and conducting permutation tests to assess dependencies between missingness and other variables.

**Key Findings:**
- Explanation of NMAR columns and their potential impact on analysis.
- Results of permutation tests showing which columns' missingness depends on other features.

The missingness matrix  provides an overview of missing patterns in a manageable format by visualizing a sample of the data. This plot helps us understand the structure of missingness and guides our analysis in determining whether data is missing at random or due to specific factors.

**Missingness Matrix:**
![Missingness Matrix Thumbnail](assets/missingness_matrix_thumbnail.svg)

[View Interactive Plot](assets/missingness_matrix.html)

This thumbnail provides a quick overview of missing data patterns in the dataset. Click the thumbnail to explore the full interactive plot and gain deeper insights into the data structure.

### 4. Hypothesis Testing
In this section, we state the hypotheses tested during the project, including the statistical tests used, significance levels, and conclusions drawn. We explored whether recipes with more than 10 ingredients have significantly different preparation times.

**Hypotheses:**
- Null Hypothesis: No significant difference in average preparation times between recipes with more than 10 ingredients and those with 10 or fewer.
- Alternative Hypothesis: Significant difference in preparation times exists between the two groups.

**Results:**
- Report on test statistic, p-value, and conclusion.

### 5. Framing a Prediction Problem
This section clearly defines the prediction task as a regression problem to predict the average rating of recipes. It outlines the response variable, chosen evaluation metrics, and the rationale for selecting these features based on what is known at the time of prediction.

**Prediction Problem:**
- **Type:** Regression
- **Response Variable:** `rating`
- **Evaluation Metric:** RMSE (Root Mean Squared Error) due to its sensitivity to prediction errors, which is crucial for predicting user satisfaction accurately.

### 6. Baseline Model
Describes the baseline model developed using a simple Linear Regression with basic features (`minutes`, `n_ingredients`). It reports on the preprocessing steps, the types of features used, and the initial model performance.

**Model Details:**
- **Features Used:** `minutes`, `n_ingredients`
- **Performance:** Cross-Validation RMSE ~0.71, Test Set R²: -0.0000
- **Interpretation:** Baseline performance indicates that these basic features alone do not sufficiently capture the variance in recipe ratings.

### 7. Final Model
Details the improvements made to the baseline model, including the addition of new features and the switch to Ridge regression with hyperparameter tuning. It discusses how the final model's performance compares to the baseline.

**Model Details:**
- **Added Features:** `calories`, `protein`, `description_length`
- **Model Used:** Ridge Regression
- **Hyperparameter Tuning:** Conducted using GridSearchCV with optimal `alpha = 1.0`
- **Performance Improvement:** Final Model Test Set RMSE: 0.7255, indicating some improvement.

### 8. Fairness Analysis
This section assesses whether the final model performs differently across different recipe groups, specifically comparing "healthy" versus "non-healthy" recipes. A permutation test is used to determine if the observed differences are statistically significant.

**Fairness Analysis Results:**
- **Groups Compared:** Healthy vs. Non-Healthy recipes
- **Evaluation Metric:** RMSE
- **Hypotheses:**
  - Null: The model is fair with similar RMSE for both groups.
  - Alternative: The model is unfair with different RMSE for the groups.
- **Results:** Observed RMSE Difference: 0.0055, P-value: 0.7090. Conclusion: Fail to reject the null hypothesis; the model appears fair between the groups.

