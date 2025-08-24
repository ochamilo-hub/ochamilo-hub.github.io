---
layout: post
author: Name
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background
Company Overview
Sephora is a global leader in the beauty retail industry, offering thousands of products across cosmetics, skincare, hair care, and fragrances. Since launching its online store in 1999, Sephora has catered to a diverse customer base with varying preferences shaped by individual characteristics such as skin tone, skin type, hair color, and eye color.

Project Motivation
Imagine scrolling through hundreds of products on Sephora’s website—each promising different benefits and priced across a wide spectrum. For beauty and skincare shoppers, personalized insights are invaluable. Questions like “Which product suits my skin type?” or “Is this worth the price?” often arise. In such moments, user ratings and reviews become critical tools for informed decision-making.

However, Sephora’s rating system is simplified to a 5-star scale, which doesn’t reveal the underlying factors that influence customer satisfaction. This sparked a curiosity: What drives a user to rate a product highly or poorly? Beyond performance, could brand, exclusivity, or popularity play a role?

Main Business Goal
The overarching goal of this project is to enhance customer satisfaction, uncover customer preferences, and inform product development decisions by leveraging data-driven insights.

Business Objective
To achieve this, the project aims to:

Predict customer ratings based on product features and customer demographics (skin tone, eye color, skin type, hair color).
Identify the bottom 20 products with predicted poor ratings for further review or improvement.
Dataset Overview
The dataset used for this analysis is titled “Sephora Products and Skincare Reviews”, sourced from Kaggle. It contains information on over 8,000 products scraped from Sephora’s online store by Nady Inky using a Python scraper in March 2023. The dataset includes product attributes, customer reviews, and ratings, providing a rich foundation for predictive modeling and trend analysis.

## Work Accomplished
By understanding the factors that influence product ratings, Sephora can:

Offer more personalized product recommendations.
Improve product development and marketing strategies.
Proactively address low-performing products to enhance overall customer experience.

### Data Preparation
1. Overview
A comprehensive data cleaning and preparation process was conducted to ensure the dataset was suitable for predictive modeling. The goal was to enhance data quality, reduce noise, and retain only relevant features for predicting customer ratings.

2. Data Cleaning Highlights
Using Pandas and NumPy, the following steps were performed:

    -Data Import & Consolidation: Multiple review files and product information datasets were imported and concatenated. An irrelevant column containing only line numbers was dropped.
  
    -Structural Fixes: Column names and formats were standardized for consistency.
  
    -Unnecessary Data Removal: Fields not contributing to the rating prediction task were removed.

3. Data Filtering & Deduplication
Temporal Filtering: Only reviews from 2021 and 2022 were retained to ensure consistency and relevance. Data from 2023 was excluded due to incomplete annual coverage.
Duplicate Removal: 523 duplicate rows were identified and removed based on a combination of author ID, timestamp, review text, and product ID. This accounted for less than 0.1% of the dataset.

5. Handling Missing Values
Demographic Fields: Missing values (<5%) were imputed using the most frequent values from the full 16-year dataset.
Helpfulness Score: With ~50% missing values, this field was imputed using the mean.
Dropped Fields:
Review Title: Not essential for rating prediction.
Tertiary Category: Replaced by Secondary Category or Product Name.
Size, value_price_usd, sale_price_usd, child_min_price, child_max_price, variation_value, and variation_type: Dropped due to redundancy or irrelevance.

7. Standardization & Grouping
Categorical Standardization: Fields like Eye Color were cleaned by merging semantically identical values (e.g., "Gray" and "grey").
Skin Tone Grouping: Consolidated into four categories — Light, Medium, Tan, and Dark — to reduce sparsity and improve model performance.

9. Sentiment Score Integration
A sentiment score was derived from the review text to quantify emotional tone. This feature helps the model interpret customer feedback beyond structured fields, with positive sentiment often correlating with higher ratings.

11. Outlier Treatment
Price (USD): Outliers (e.g., $1900) were addressed using log transformation to reduce skew while preserving distribution shape.
Feedback Counts: Extreme values were managed using capping techniques to maintain model stability.

13. Correlation Analysis
A correlation matrix was generated post-cleaning:
is_recommended showed the strongest correlation with rating.
love_count and price_usd had weak correlations.
Redundant feedback count columns were identified for removal to avoid multicollinearity.

15. Encoding & Feature Selection
After cleaning and transformation, 11 relevant columns were selected from an initial 27. These features capture both customer demographics and product attributes, forming the foundation for the rating prediction model.

### Modelling
To predict customer ratings for Sephora products using structured product features and customer demographics. Three regression models were evaluated using GridSearchCV for hyperparameter tuning:

1. Linear Regression (Baseline)
2. XGBoost Regression
3. Random Forest Regression

1. Linear Regression (Baseline Model)
Purpose: Establish a baseline for model performance using a simple linear approach.

Method: Ordinary Least Squares Regression
Hyperparameter Tuning: Not applicable (no tunable parameters in basic linear regression)
Performance Metrics:
Mean Squared Error (MSE): 1.197
R-squared (R²): 0.005
Interpretation:
The model explains less than 1% of the variance in ratings, indicating that linear relationships alone are insufficient for capturing the complexity of customer rating behavior.

2. XGBoost Regression
Purpose: Leverage gradient boosting to model complex, non-linear relationships.

Hyperparameter Tuning via GridSearchCV:
learning_rate: 0.1
n_estimators: 200
Performance Metrics:
MSE: 1.1182
R-squared (R²): 0.0704
Interpretation:
XGBoost outperformed the baseline, capturing more variance and reducing error. The model benefits from boosting and handles feature interactions well, making it suitable for this task.

3. Random Forest Regression
Purpose: Use ensemble learning to reduce overfitting and improve generalization.

Hyperparameter Tuning via GridSearchCV:
max_depth: 10
n_estimators: 200
Performance Metrics:
MSE: 1.13
R-squared (R²): 0.06
Interpretation:
Random Forest also improved upon the baseline, though slightly less effective than XGBoost. It provides robustness and interpretability, especially useful for understanding feature importance.


### Evaluation
While all models show modest predictive power, XGBoost Regression demonstrated the best performance in terms of both error reduction and variance explanation. Further improvements may be achieved by:

a. Incorporating additional features (e.g., review length, brand popularity)

b. Using ensemble stacking

c. Applying advanced NLP techniques to review text

## Recommendation and Analysis
Explain the analysis and recommendations

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
