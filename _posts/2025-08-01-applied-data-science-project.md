---
layout: post
author: Tan Chin Cheah
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background

<img width="1200" height="544" alt="image" src="https://github.com/user-attachments/assets/a0de52c3-3829-4302-b320-7ecf552e8650" />

## Company Overview

Sephora is a global leader in the beauty retail industry, offering thousands of products across cosmetics, skincare, hair care, and fragrances. Since launching its online store in 1999, Sephora has catered to a diverse customer base with varying preferences shaped by individual characteristics such as skin tone, skin type, hair color, and eye color.

## Project Motivation

Imagine scrolling through hundreds of products on Sephora’s website—each promising different benefits and priced across a wide spectrum. For beauty and skincare shoppers, personalized insights are invaluable. Questions like “Which product suits my skin type?” or “Is this worth the price?” often arise. In such moments, user ratings and reviews become critical tools for informed decision-making.

However, Sephora’s rating system is simplified to a 5-star scale, which doesn’t reveal the underlying factors that influence customer satisfaction. This sparked a curiosity: What drives a user to rate a product highly or poorly? Beyond performance, could brand, exclusivity, or popularity play a role?

## Main Business Goal

The overarching goal of this project is to enhance customer satisfaction, uncover customer preferences, and inform product development decisions by leveraging data-driven insights.

## Business Objective

To achieve this, the project aims to Predict customer ratings based on product features and customer demographics (skin tone, eye color, skin type, hair color).
Identify the bottom 20 products with predicted poor ratings for further review or improvement.
Dataset Overview

The dataset used for this analysis is titled “Sephora Products and Skincare Reviews”, sourced from Kaggle. It contains information on over 8,000 products scraped from Sephora’s online store by Nady Inky using a Python scraper in March 2023. The dataset includes product attributes, customer reviews, and ratings, providing a rich foundation for predictive modeling and trend analysis.

## Work Accomplished

By understanding the factors that influence product ratings, Sephora can work with their brand partner to proactively address low-performing products to enhance overall customer experience.

1. Data-Driven Product Flagging

Identified the bottom 20 underperforming products based on predicted customer ratings across demographic factors (skin tone, skin type, eye color, hair color), price positioning, and ingredient profiles.

2. Root-Cause Analysis

Performed segmentation analysis to uncover which customer demographic groups oily skin type, tan and dark skin tones reported lower satisfaction.

3. Analyzed pricing tiers and ingredient compositions that correlate with poor ratings.

Highlighted formulation risks (harsh or less-preferred ingredients).

Flagged misaligned price-value perception (products priced high but delivering lower customer satisfaction).

4. Strategic Business Recommendations

Suggested skin-type-specific variants, ingredient optimization, and customer testing strategies.

Insights designed to help Sephora influence brand partners to reformulate or reposition products.

### Data Preparation

1. Overview
A comprehensive data cleaning and preparation process was conducted to ensure the dataset was suitable for predictive modeling. The goal was to enhance data quality, reduce noise, and retain only relevant features for predicting customer ratings.

2. Data Cleaning Highlights
Using Pandas and NumPy, the following steps were performed:

•	Data Import & Consolidation: Multiple review files and product information datasets were imported and concatenated. An irrelevant column containing only line numbers was dropped.

•	Structural Fixes: Column names and formats were standardized for consistency. 

•	Unnecessary Data Removal: Fields not contributing to the rating prediction task were removed.

<img width="519" height="238" alt="image" src="https://github.com/user-attachments/assets/a18b01e9-0bcc-487e-8532-ced801e0a394" />

3. Data Filtering & Deduplication
•	Temporal Filtering: Only reviews from 2021 and 2022 were retained to ensure consistency and relevance. Data from 2023 was excluded due to incomplete annual coverage.

•	Duplicate Removal: 523 duplicate rows were identified and removed based on a combination of author ID, timestamp, review text, and product ID. This accounted for less than 0.1% of the dataset.

<img width="905" height="154" alt="image" src="https://github.com/user-attachments/assets/f7ba294f-2170-43de-b396-1e401152d910" />

4. Handling Missing Values
•	Demographic Fields: Missing values (<5%) were imputed using the most frequent values from the full 16-year dataset.

•	Helpfulness Score: With ~50% missing values, this field was imputed using the mean. (This field was ultimately dropped from the analysis because it showed only weak correlation.d)

Dropped Fields:

•	Review Title: Not essential for rating prediction.

•	Tertiary Category: Replaced by Secondary Category or Product Name.

•	Size, value_price_usd, sale_price_usd, child_min_price, child_max_price, variation_value, and variation_type: Dropped due to redundancy or irrelevance.

<img width="531" height="140" alt="image" src="https://github.com/user-attachments/assets/495317e6-9aa2-4c2b-b540-583c72a4935d" />

5. Standardization & Grouping
•	Categorical Standardization: Fields like Eye Color were cleaned by merging semantically identical values (e.g., "Gray" and "grey").

•	Skin Tone Grouping: Consolidated into four categories — Light, Medium, Tan, and Dark — to reduce sparsity and improve model performance.

<img width="458" height="137" alt="image" src="https://github.com/user-attachments/assets/63f069a6-1ad0-4950-8f5d-27cf0f14671d" />

<img width="503" height="129" alt="image" src="https://github.com/user-attachments/assets/a5c59b7b-a0b4-4bab-a764-d4b97dde23ae" />

6. Sentiment Score Integration
•	A sentiment score was derived from the review text to quantify emotional tone. This feature helps the model interpret customer feedback beyond structured fields, with positive sentiment often correlating with higher ratings.

<img width="610" height="110" alt="image" src="https://github.com/user-attachments/assets/93b3daa6-9aab-483b-a7c1-961c6ef992d9" />


7. Outlier Treatment
•	Price (USD): Outliers (e.g., $1900) were addressed using log transformation to reduce skew while preserving distribution shape.

•	Feedback Counts: Extreme values were managed using capping techniques to maintain model stability.

<img width="550" height="281" alt="image" src="https://github.com/user-attachments/assets/fb1e4e3e-552d-4234-9470-4b1c4ad7f58b" />
<img width="511" height="331" alt="image" src="https://github.com/user-attachments/assets/4b6a1758-4a2d-4797-9439-b1485039acf0" />


8. Correlation Analysis
A correlation matrix was generated post-cleaning:
•	is_recommended showed the strongest correlation with rating.

•	love_count and price_usd had weak correlations.

•	Redundant feedback count columns were identified for removal to avoid multicollinearity.

<img width="566" height="272" alt="image" src="https://github.com/user-attachments/assets/f7e8414c-1936-4267-9df8-0ce49b65f78b" />


9. Encoding & Feature Selection
After cleaning and transformation, 11 relevant columns were selected from an initial 27. These features capture both customer demographics and product attributes, forming the foundation for the rating prediction model.

•	Product Features: rating, loves_count, price_usd, and sentiment_score,

•	Customer Demographics: skin_tone_grouped, eye_color, skin_type, and hair_color.

•	Interaction/Review Features: is_recommended, total_neg_feedback_count, and total_pos_feedback_count

<img width="1792" height="396" alt="image" src="https://github.com/user-attachments/assets/eed767cc-1cd3-4b37-a34a-e94cd307681a" />

**Dataset Quality Check**

<img width="586" height="316" alt="image" src="https://github.com/user-attachments/assets/0ca928a1-e593-43cc-898b-6ad860c763c2" />


### Modelling
For this project, 4 regression models were selected because the primary objective was to predict continuous numerical outcomes—customer ratings. Regression algorithms are designed to estimate values along a continuous scale, ideal for tasks like rating prediction

Except Rick regression doesn’t require hypeparameter tunning, the 3 models underwent using GridSearchCV to optimize performance. The evaluation was based on two key metrics: Mean Squared Error (MSE) and R-squared (R²). Lower MSE indicates better prediction accuracy, while higher R² reflects stronger explanatory power.

The dataset was then split into training and testing sets in an 80/20 ratio. 

**1. Riedge Regression (Baseline Model)**

•	Purpose: A baseline linear model. It’s simple, interpretable, and effective for datasets with multicollinearity, it has L2 regularization mechanism, helping reduce model complexity while retaining all features.
•	Method: Ordinary Least Squares Regression
•	Hyperparameter Tuning: Not applicable (no tunable parameters in basic linear regression)
•	Performance Metrics:
•	Mean Squared Error (MSE): 0.3559
•	R-squared (R²): 0.7041

•	Interpretation:
Indicating decent performance but limited ability to capture complex patterns.

**2. XGBoost Regression**

•	Purpose: XGBoost Regression is a powerful gradient boosting ensemble algorithm known for its high accuracy and efficiency. It handles complex relationships well and is robust to overfitting with proper tuning, leverage gradient boosting to model complex, non-linear relationships, 
•	Hyperparameter Tuning via GridSearchCV:
•	learning_rate: 0.1
•	n_estimators: 200
•	Performance Metrics:
•	MSE: 0.3273
•	R-squared (R²): 0.7041

•	Interpretation:
XGBoost outperformed the baseline, capturing more variance and reducing error. The model benefits from boosting and handles feature interactions well, making it suitable for this task.

**3. Random Forest Regression**

•	Purpose: it can capture complex customer-product interactions, is robust against noisy review data, works seamlessly with mixed feature types, and provides interpretable insights •	that are valuable for product strategy.
•	Hyperparameter Tuning via GridSearchCV:
•	max_depth: 10
•	n_estimators: 200
•	Performance Metrics:
•	MSE: 0.3317
•	R-squared (R²): 0.7242

•	Interpretation:
Random Forest also improved upon the baseline, though slightly less effective than XGBoost. It provides robustness and interpretability, especially useful for understanding feature importance.

**4. HistGradientBoosting Regression**

•	Purpose: Newer boosting ensemble method optimized for speed and scalability. It’s particularly effective with large datasets and supports missing values natively.
learning_rate: 0.1
•	Max_leaf_nodes: 50
•	Performance Metrics:
•	MSE: 0.3289
•	R-squared (R²): 0.7266

•	Interpretation:
Closely matching XGBoost’s performance.

Here’s a comparison of the four models. As you can see, XGBoost delivered the best performance overall, with the lowest error and highest R-squared value. 

<img width="255" height="74" alt="image" src="https://github.com/user-attachments/assets/ba7c9616-c5f2-4165-9504-6a428adcb1d7" />



<img width="284" height="278" alt="image" src="https://github.com/user-attachments/assets/f17ac9ad-693c-4b45-9746-c58dda733443" />


### Evaluation

<img width="613" height="339" alt="image" src="https://github.com/user-attachments/assets/02e982db-b891-4679-8d78-a83f6bf8aa54" />

### Analysis of Learning Curves

Learning curves show how the model's performance changes with increasing training data size.

•	For Ridge Regression, the training and validation error converge relatively quickly, indicating that adding more data might not significantly improve performance with this model.

•	For XGBoost Regression, the training error decreases and the validation error decreases as the training set size increases. The curves are still somewhat separated, suggesting that more data could potentially improve performance.

•	For Random Forest Regression, similar to XGBoost, the training error decreases and validation error decreases with more data. There is a gap between the curves, indicating potential for improvement with more data.

•	For HistGradientBoosting Regression, the learning curves show a similar trend to XGBoost and Random Forest, with a gap between training and validation error, suggesting that more data might be beneficial.

<img width="625" height="349" alt="image" src="https://github.com/user-attachments/assets/8c869ff1-68e4-4e19-b126-f765da61a0d9" />

### Analysis of Validation Curves

Validation curves show how the model's performance changes with different hyperparameter values.

•	For Ridge Regression (alpha), the validation error is relatively stable across a wide range of alpha values, with a slight increase at very high alpha values.

•	For XGBoost Regression (n_estimators), the training error decreases and validation error decreases as the number of estimators increases. The validation error seems to plateau after a certain number of estimators, suggesting diminishing returns from adding too many estimators.

•	For Random Forest Regression (n_estimators), the training error decreases and validation error is relatively stable as the number of estimators increases. The validation error does not show significant improvement with more estimators beyond a certain point.

•	For HistGradientBoosting Regression (max_leaf_nodes), the training error decreases and validation error decreases as the number of max_leaf_nodes increases. The validation error seems to continue decreasing with increasing max_leaf_nodes within the tested range

### Summary of Findings 

Based on the learning curves, all tree-based models (XGBoost, Random Forest, HistGradientBoosting) show potential for improvement with more training data, as indicated by the gap between training and validation error. Ridge Regression's performance seems less sensitive to the amount of training data.
The validation curves provide insights into the impact of key hyperparameters. For XGBoost and HistGradientBoosting, increasing the number of estimators or max_leaf_nodes generally improves performance up to a point. For Random Forest, the number of estimators has less impact on validation error within the tested range. For Ridge, the alpha parameter's impact on performance is minimal within the tested range.

•	Overall: XGBoost and HistGradientBoosting exhibit promising characteristics in both learning and validation curves, suggesting they are better at capturing the underlying data patterns compared to Ridge and potentially Random Forest within the explored hyperparameter ranges. 

XGBoost with the lowest error and highest R-squared value and exhibited promising characteristics in both learning and validation curve was selected as the final model for generating predicted ratings and identifying the bottom 20 products for review.

We used the beset model and predicted ratings for all products. We then identified the bottom 20 product based on average predicted ratings. These products are flagged for review, offering insights for product improvement 
<img width="612" height="338" alt="image" src="https://github.com/user-attachments/assets/4ff87bf8-eb01-4ff5-851d-fc120ba30bfc" />

## Feature Importance from Best XGBoost Model

<img width="587" height="296" alt="image" src="https://github.com/user-attachments/assets/c13eb3ee-e55e-41d7-b0e0-a0197bd687d4" />

Using the best-performing XGBoost model, feature importance analysis identified 'is_recommended' as the most influential predictor of customer ratings. This finding is consistent with our earlier correlation analysis during preprocessing stage, where recommendation likelihood was strongly aligned with rating behavior. Customers who would recommend a product generally provided higher ratings. In contrast, variables such as demographics and price displayed weak correlations, indicating limited predictive power.

## Recomendation and Analysis

Let’s see how different customer demographics interact with the bottom 20 products, we segmented by skin type, skin tone, eye color, and hair color. 

<img width="617" height="341" alt="image" src="https://github.com/user-attachments/assets/161e6fe3-e812-405b-8f00-54733a15ea80" />

•	Starting with skin type, the distribution of predicted ratings across categories such as oily, dry, combination, and normal. oily skin shows a lower median and a narrow spread, this suggests that customers with oily skin consistently rate these products poorly. This could indicate a mismatch between product formulation and the needs of this skin type. 

Next, we looked at skin tone groups—Dark, Light, Medium, and Tan. 

•	Medium & Light skin tones has Wider spread, with some outliers as high as ~4.5.
this Suggests these Medium & Light have more polarized experiences, some users really dislike the products, but a minority rate them much higher. 

•	Tan & Dark skin tones' Ratings are consistently low (~1.5–1.7). This shows uniform dissatisfaction, with little variation. Likely these products don’t cater well to deeper tones. one of the bottom product is Play, Mineral spf Stick should be look into to help to serve better for underserved skin tone group.

Although eye color and hair color may not directly influence product performance, their box plots can still reveal interesting trends. 

•	Brown and Hazel eye colors appear to have lower median predicted ratings for the bottom 20 products.

•	Black hair: Consistently low (~1.5–1.7), little variation. Products underperform here.

<img width="625" height="349" alt="image" src="https://github.com/user-attachments/assets/67911379-d01a-497b-89a3-28e51d46213e" />

Let’s look at other product feature like Price and ingredients 

•	Several products priced above $250–$300 show predicted ratings below 2.0, indicating poor perceived value. This is especially noticeable in categories like Wellness and High Tech Tools.

•	Products in the Treatments category tend to have higher predicted ratings even at lower price points, suggesting strong value perception and customer satisfaction.

•	Wellness, High Tech Tools, and Value & Gift Sets show clusters of low ratings, regardless of price.

•	Masks and Moisturizers show mixed performance, indicating variability in formulation or customer fit. 

I also completed an ingredient-level analysis of the bottom 20 products. the focus was on identifying commonly used ingredients that may be contributing to poor ratings among sensitive demographics.


## Recommendations:
Since Sephora is a distributor and not a manufacturer, the recommendations should focus on influencing brand partners and curating product assortments 

Sephora to share the Bottom 20 products insights with brand partners, work with them:

1. Ingredient Optimization: Reduce or replace ingredients like phenoxyethanol or harsh exfoliants in products targeted at sensitive demographics. Like recommended by dermatologists' product La Roche-Posay

2. Targeted Testing: Conduct user trials with affected groups to validate reformulations and ensure improved performance. As well wellness and high-tech products makers (toning tool), to provide demo or testing especial in roadshows 

3. R&D Reformulation : encourage partner to works with dermatologists with reassess active ingredients for efficacy and compatibility with target demographics, re-introduce skin-type-specific variants example: New foundation shades, addressing underserved skin tones.


## AI Ethics
1. Privacy
The dataset includes customer demographics such as skin tone, eye color, and hair color. While these features are useful for personalization, they are also sensitive and potentially identifiable. Data must be collected and processed under anonymized and stripped of personally identifiable information (PII).Avoid storing or sharing raw data that includes user IDs or timestamps unless necessary and secure. Comply with data protection regulations such as GDPR or PDPA (Singapore), especially when handling user-generated content.

2. Fairness
Using demographic features to predict ratings may unintentionally reinforce biases. For example, if certain skin tones are associated with lower ratings due to historical bias in product formulation or marketing, the model may perpetuate this unfairness.  skin tone values were grouped into broader categories: Light, Medium, Tan, and Dark—through binning. This approach reduces granularity that could lead to biased predictions or racial profiling, while still preserving meaningful variation for modeling. The goal is to ensure that demographic features are used to enhance inclusivity, not to reinforce stereotypes or discrimination.

3. Accuracy
The best-performing model (XGBoost) achieved an R² of only 0.0704, indicating limited predictive power. Relying on such models for business decisions could lead to inaccurate conclusions.Use predictions as supporting insights, not definitive judgments. Continuously monitor and retrain models with updated data to improve accuracy.

4. Accountability
Decisions based on model outputs such as flagging products for review must be traceable and justifiable.Maintain audit trails of model development, training data, and decision logic.
Ensure that human oversight is present in all critical decision-making processes. Document model assumptions, limitations, and evaluation metrics.

5. Transparency
Provide clear documentation and visualizations of model behavior.


## Source Codes and Datasets
sharing in Drive because dataset files are too big to be uploaded in github, which the repository site has limited to 25mb only
Goodle Drive https://drive.google.com/drive/folders/14RaCVic6QR4-hDZAZcCu9ZF6GHfmWGKp 

ipynb files uploaded in github repository as below
1. preprocessing
2. modeling and evaluation
https://github.com/ochamilo-hub/ITD214-Data-science-project/upload
