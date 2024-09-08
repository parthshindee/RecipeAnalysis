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
[View Interactive Plot](assets/missingness_matrix.html)

This thumbnail provides a quick overview of missing data patterns in the dataset. Click the thumbnail to explore the full interactive plot and gain deeper insights into the data structure.

### 4. Hypothesis Testing
I explored whether recipes with more than 10 ingredients have significantly different preparation times.

**Hypotheses:**
- **Null Hypothesis:** There is no significant difference in average preparation times between recipes with more than 10 ingredients and those with 10 or fewer.
- **Alternative Hypothesis:** There is a significant difference in preparation times between the two groups.

**Results:**
- **Observed Difference in Means:** -4.93 minutes
- **P-value:** 0.8310
- **Conclusion:** Fail to reject the null hypothesis; there is no significant difference in average preparation times between recipes with more than 10 ingredients and those with 10 or fewer. This suggests that the number of ingredients does not substantially impact the preparation time.

### 5. Framing a Prediction Problem
**Prediction Problem:**
The goal of this project is to predict the average rating of a recipe based on its features, such as preparation time, number of ingredients, nutritional content, and user-provided tags. This is a regression problem, as the response variable (`rating`) is continuous.

**Problem Type:** Regression

**Response Variable:** `average_rating` — the average user rating of a recipe.

**Features Used:**
- `minutes`: Time required to prepare the recipe.
- `n_ingredients`: Number of ingredients used.
- `calories`, `protein`, and other nutritional values extracted from the `nutrition` column.
- `description_length`: Length of the recipe's user-provided description.
- `tags`: User-provided tags describing the recipe, processed as categorical features.

**Evaluation Metric:**
- **Root Mean Squared Error (RMSE):** Chosen due to its sensitivity to large errors, RMSE is a suitable metric for this regression problem because it penalizes significant deviations from actual ratings, making it ideal for measuring user satisfaction.

### 6. Baseline Model
**Baseline Model Approach:**
The baseline model uses a simple Linear Regression model with basic features to provide a reference point for model performance.

**Features Used:**
- `minutes`
- `n_ingredients`

**Preprocessing:**
- **Imputation:** Used mean imputation for missing values.
- **Scaling:** Standardized numerical features using StandardScaler.

**Model Pipeline:**
A pipeline was created using `sklearn` to handle preprocessing and modeling in one streamlined process.

**Performance:**
- **Cross-Validation RMSE:** Approximately 0.71
- **Test Set R²:** -0.0000

**Interpretation:** The baseline model's performance indicates that these basic features alone do not sufficiently capture the variance in recipe ratings, as evidenced by the near-zero R² value.

### 7. Final Model
The final model builds upon the baseline by incorporating additional features and applying Ridge regression with hyperparameter tuning to improve performance.

**Features Added:**
- `calories`, `protein`: Extracted from the `nutrition` column for deeper insights into recipe quality.
- `description_length`: Length of the description text to gauge the potential influence of detailed descriptions on ratings.
- **Categorical Feature Encoding:** Processed `tags` with OneHotEncoder to include user-provided tags.

**Model Pipeline and Tuning:**
A more sophisticated pipeline was created to handle both preprocessing and model fitting, with hyperparameters tuned using GridSearchCV.

**Performance Improvement:**
- **Best Hyperparameters:** `alpha = 1.0`
- **Final Model Test Set RMSE:** 0.7255

**Interpretation:** The final model shows improvement over the baseline by incorporating more features and using regularization to manage complexity, resulting in a slightly better RMSE.

### 8. Fairness Analysis
**Objective:**
To assess whether the final model performs fairly across different recipe groups, specifically comparing "healthy" versus "non-healthy" recipes.

**Groups Compared:**
- **Healthy Recipes:** Recipes tagged with 'healthy'.
- **Non-Healthy Recipes:** Recipes without the 'healthy' tag.

**Evaluation Metric:**
- **RMSE:** Used to measure prediction errors for each group.

**Hypotheses:**
- **Null Hypothesis:** The model is fair with similar RMSE for both healthy and non-healthy recipes.
- **Alternative Hypothesis:** The model is unfair, with differing RMSE between the two groups.

**Results:**
- **Observed RMSE Difference:** 0.0055
- **P-value:** 0.7090

**Conclusion:** Fail to reject the null hypothesis; the model appears fair between the groups with similar RMSE for both healthy and non-healthy recipes.

