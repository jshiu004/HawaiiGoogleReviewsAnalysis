# What Drives Restaurant Prices in Hawaii?
## By Justin Shiu 

# Introduction

## General Introduction
Hawaii is widely known as one of the most expensive states in the United States, both for residents and visitors. As a major tourist destination, food plays an important role in the overall experience of visiting the islands. When choosing where to eat, many people rely on Google and Google Maps to compare restaurants, read reviews, and estimate prices.

Because Google labels restaurants using price categories, I wanted to explore what factors may influence how a restaurant is classified. Features such as ratings, number of reviews, restaurant category, location, and distance from popular tourist areas may all help explain why some restaurants are labeled as more expensive than others. By analyzing these patterns, this project aims to better understand what characteristics are associated with higher restaurant price categories in Hawaii.

The central question I am interested in is: 
**What factors influence a restaurant’s Google price category in Hawaii?**

## Dataset Introduction
This dataset contains a detailed summary of establishments across the Hawaiian Islands, including common Google Maps information such as business name, geographic coordinates, average rating, and number of reviews. The dataset has **21,507 rows and 15 columns**, providing a wide range of information about each establishment. For this project, I focus on the columns most relevant to understanding and predicting a restaurant’s Google price category.

Here is a brief description of the relevant columns:

- **name**: The name of the establishment.
- **address**: The physical address of the establishment.
- **latitude**: The latitude coordinate of the establishment’s location.
- **longitude**: The longitude coordinate of the establishment’s location.
- **category**: The type of establishment or business category.
- **avg_rating**: The establishment’s average Google rating, ranging from 1 to 5.
- **num_of_reviews**: The total number of Google reviews the establishment has received.
- **price**: The Google price category of the establishment, ranging from `$` to `$$$$`.
- **state**: The current status of the restaurant, indicating whether it is permanently closed or still open.
- **hours**: The listed operating hours of the establishment.

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning
The first step in cleaning the data was to filter the DataFrame so that it only included food-related establishments, rather than all types of locations. To do this, I filtered the **category** column to keep only establishments with food-related categories, such as restaurants, cafes, bars, and dessert shops.

Next, I filtered the DataFrame further by removing rows where **state** indicated that the establishment was permanently closed. This ensured that the analysis only included businesses that are still operating.

To make the price variable easier to work with, I created a new column called **price_num**, which converts the dollar-sign price category into a numeric value from 1 to 4.

I then used the **latitude** and **longitude** coordinates to identify which Hawaiian island each establishment was located on. Establishments that were not located on one of the six main Hawaiian Islands — Oahu, Maui, Big Island, Kauai, Molokai, or Lanai — were removed from the dataset.

After that, I created a new column called **distance_to_nearest_beach_km**, which measures each establishment’s distance to the nearest beach using beach locations from the original dataset.

Finally, I used the **hours** column to engineer several new features related to operating hours, including **days_open_per_week**, **total_hours_open_per_week**, **avg_hours_per_day**, **open_weekend**, **opens_early**, and **closes_late**.

The final cleaned DataFrame contained the original columns **category**, **avg_rating**, **num_of_reviews**, and **price**, along with several engineered features: **price_num**, **island**, **days_open_per_week**, **total_hours_open_per_week**, **avg_hours_per_day**, **open_weekend**, **opens_early**, and **closes_late**.

Below is the head of the cleaned dataframe:
### Preview of Cleaned DataFrame

#### Core Restaurant Information

| category | avg_rating | num_of_reviews | price | price_num | island |
|:---|---:|---:|:---:|---:|:---|
| ['Coffee shop'] | 4.8 | 8 | $ | 1 | Maui |
| ['Restaurant' 'Cafe'] | 4.3 | 33 | $$ | 2 | Oahu |
| ['Cafe'] | 4.4 | 18 | $ | 1 | Oahu |
| ['Bubble tea store' 'Coffee shop'] | 4.4 | 48 | $ | 1 | Oahu |
| ['Coffee shop'] | 4.8 | 8 | $ | 1 | Maui |

#### Engineered Location and Hours Features

| distance_to_nearest_beach_km | days_open_per_week | total_hours_open_per_week | avg_hours_per_day | open_weekend | opens_early | closes_late |
|---:|---:|---:|---:|---:|---:|---:|
| 1.645 | 7 | 87.5 | 12.5 | 1 | 1 | 0 |
| 0.648 | 7 | 78.0 | 11.14 | 1 | 0 | 1 |
| 4.012 | 4 | 22.0 | 5.5 | 0 | 0 | 0 |
| 1.620 | 5 | 45.0 | 9.0 | 0 | 0 | 0 |
| 1.645 | 7 | 87.5 | 12.5 | 1 | 1 | 0 |

## Univariate Analysis

I performed univariate analysis on the **price_nums** column to see the distribution of price levels. 

<iframe
  src="assets/price_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This distribution shows that most food establishments in the dataset are labeled as either `$` or `$$`, while far fewer are labeled as `$$$` or `$$$$`. This suggests that lower-priced establishments are much more common in the dataset than higher-priced ones.

However, this does not necessarily mean that eating in Hawaii is inexpensive overall. Since the price labels are relative Google categories, a `$` or `$$` restaurant in Hawaii may still be more expensive than a similarly labeled restaurant in another state. Instead, this plot mainly shows that, within Hawaii, most listed food establishments fall into the lower Google price categories.

This imbalance is also important for modeling because there are many more examples of `$` and `$$` restaurants than `$$$` and `$$$$` restaurants. As a result, a classification model may have an easier time predicting the common price categories and may struggle to accurately predict the less common higher-price categories.

I also performed univariate analysis on the **avg_rating** column to see the distribution of average_rating. 

<iframe
  src="assets/avg_rating_violin2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This distribution shows that restaurant ratings are heavily concentrated above 3.5, with relatively few establishments receiving lower ratings. The median rating is 4.5, suggesting that most food establishments in the dataset are rated highly by Google reviewers.

However, this should be interpreted carefully because Google ratings tend to be skewed toward higher values. A high rating may indicate customer satisfaction, but it may also reflect review bias, since people who leave reviews may not represent all customers equally.

## Bivariate Analysis

Next I explored the relationship between the average ratings and price levels of food establishments.

<iframe
  src="assets/avg_rating_by_price.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The side-by-side box plots show that the median average rating tends to increase as price level increases. This suggests that **avg_rating** may be associated with a restaurant’s price category and could potentially be a useful predictor in the model.
## Interesting Aggregates

One aggregation I was interested in exploring was how average price categories differed across both restaurant type and island. To make this comparison easier, I grouped the original **category** values into broader categories: **Restaurant**, **Bar / Nightlife**, **Beverages**, **Dessert**, **Small Shops**, and **Other**.

I then created a heatmap that shows the average **price_num** for each broad category across the different Hawaiian Islands. This allows for easier comparison of whether certain types of food establishments tend to be more expensive on specific islands.

<iframe
  src="assets/avg_price_heatmap.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the heatmap, we can see that average price categories vary across both islands and broad establishment types. This suggests that **island location** may be associated with Google price category and could be a useful feature when building a predictive model.

However, the differences should be interpreted carefully because some category-island combinations may have fewer establishments than others. A higher or lower average price in one cell may be influenced by a small number of restaurants rather than a strong overall trend.

# Assessment of Missingness
## NMAR Analysis

I believe that the **price** column may be **NMAR** because whether a price value is missing could depend on the true, unobserved price category itself. For example, some establishments may not have a listed Google price category because Google does not have enough information to confidently assign one, or because certain types of establishments are less likely to display price information.

I also considered whether missingness in **price** could be related to **avg_rating**. For instance, more expensive restaurants may receive different ratings if customers feel that the food or experience does not justify the cost. However, since this relationship depends on the actual missing price value, the missingness may not be fully explained by the observed rating alone.

Because of this, I classify **price** as likely **NMAR**, meaning that the missingness may depend on the missing price value itself.

## Missingness Dependency

In this part, I ran permutation tests to test if the missingess of 'price' was dependent on any other columns.

The first test checked to see if 'price' missingness was dependent on 'latitude'

**Null Hypothesis:**  
The missingness of **price** is not related to **latitude**. In other words, the distribution of **latitude** is the same for establishments with missing and non-missing **price** values.

**Alternative Hypothesis:**  
The missingness of **price** is related to **latitude**. In other words, the distribution of **latitude** is different for establishments with missing and non-missing **price** values.

**Test Statistic:**  
The difference in mean **latitude** between establishments with missing **price** values and establishments with non-missing **price** values.

**Significance Level:**  
I used a 5% significance level, meaning that I reject the null hypothesis if the p-value is less than 0.05.

Here we have a histogram showing the test_statistics over 10000 iterations.
<iframe
  src="assets/latitude_permutation_test_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the permutation test, we observed a p-value of approximately 0.08. Since the p-value is greater than 0.05, we fail to reject the null hypothesis that the distribution of **latitude** is the same for establishments with missing and non-missing **price** values. This suggests that the missingness of **price** is likely unrelated to **latitude**



The second test checked to see if 'price' missingness was dependent on 'avg_rating'

**Null Hypothesis:**  
The missingness of **price** is not related to **avg_rating**. In other words, the distribution of **avg_rating** is the same for establishments with missing and non-missing **price** values.

**Alternative Hypothesis:**  
The missingness of **price** is related to **avg_rating**. In other words, the distribution of **avg_rating** is different for establishments with missing and non-missing **price** values.

**Test Statistic:**  
The difference in mean **avg_rating** between establishments with missing **price** values and establishments with non-missing **price** values.

**Significance Level:**  
I used a 5% significance level, meaning that I reject the null hypothesis if the p-value is less than 0.05.

Here we have a histogram showing the test_statistics over 10000 iterations.
<iframe
  src="assets/permutation_test_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the permutation test, we observed a p-value of approximately 0. Since the p-value is approximately 0, we reject the null hypothesis that the distribution of **avg_rating** is the same for establishments with missing and non-missing **price** values. This suggests that the missingness of **price** is likely **MAR** because whether **price** is missing appears to depend on an observed variable, **avg_rating**.


# Hypothesis Testing

Oahu is the most populated and one of the most visited Hawaiian Islands. Since heavy tourism can often influence restaurant prices, I wanted to test whether restaurants on Oahu have different Google price categories compared to restaurants on the other Hawaiian Islands.

**Null Hypothesis:**  
There is no difference in the average **price_num** between restaurants on Oahu and restaurants on the other Hawaiian Islands.

**Alternative Hypothesis:**  
There is a difference in the average **price_num** between restaurants on Oahu and restaurants on the other Hawaiian Islands.

**Test Statistic:**  
The mean **price_num** for restaurants on Oahu minus the mean **price_num** for restaurants on all other islands.

**Significance Level:**  
I used a 5% significance level, meaning that I reject the null hypothesis if the p-value is less than 0.05.

Here we have a histogram showing the test_statistics over 10000 iterations.

<iframe
  src="assets/oahu_price_permutation_test_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the permutation test, I observed a p-value of approximately 0. Therefore, we reject the null hypothesis that there is no difference in average **price_num** between restaurants on Oahu and restaurants on the other Hawaiian Islands.

Interestingly, the observed difference was negative which suggests that, on average, restaurants on Oahu have slightly lower Google price categories than restaurants on the other islands. This was somewhat surprising because Oahu is highly populated and heavily visited by tourists, so I initially expected Oahu restaurants to have higher price categories. One possible explanation is that Oahu has a larger and more diverse food scene, including many lower-priced casual restaurants, cafes, and takeout spots, which may lower its average price category.


# Framing a Prediction Problem

For my prediction model, I will perform **multiclass classification** using the **price_num** column as the target variable. I chose this approach because my goal is to predict the Google price category of food establishments across the Hawaiian Islands based on features such as category, rating, review count, island, distance to the nearest beach, and operating hours.

Since my dataset contains far fewer restaurants with **price_num** values of 3 and 4, I combined these two groups into a single category. This helps reduce class imbalance and gives the model more examples to learn from for higher-priced establishments.

The metric I will use to evaluate my model is **precision**. I chose precision because the target variable is imbalanced, with many more lower-priced establishments than higher-priced ones. In this situation, accuracy alone can be misleading because a model could achieve a high accuracy score by mostly predicting the more common lower-price categories. Precision is useful because it measures how often the model is correct when it predicts a particular price category, making it a better way to evaluate the reliability of the model’s predictions.

# Baseline Model


For my predictive model, I used a **RandomForestClassifier** to perform multiclass classification on the **price_num** column. The goal of this model is to predict the Google price category of food establishments across the Hawaiian Islands using features related to rating, popularity, location, and operating hours.

The features used in my model were:

features = [
    "avg_rating",
    "num_of_reviews",
    "island",
    "distance_to_nearest_beach_km",
    "days_open_per_week",
    "total_hours_open_per_week",
    "avg_hours_per_day",
    "open_weekend",
    "opens_early",
    "closes_late"
]

There were 6 quantitative features: avg_rating, num_of_reviews, distance_to_nearest_beach_km, days_open_per_week, total_hours_open_per_week, and avg_hours_per_day. These features represent numerical measurements such as ratings, review counts, distance, and operating hours.

There were 4 nominal categorical features: island, open_weekend, opens_early, and closes_late. Although open_weekend, opens_early, and closes_late are encoded as 0 and 1, they represent yes/no categories rather than true numerical measurements. The island column was encoded using one-hot encoding so that the RandomForestClassifier could use it as a feature.

There were 0 ordinal features in this model. While the target variable price_num is ordinal because the values represent increasing price categories, it is not counted as a feature because it is the variable being predicted.

Here is the classification report:


|              |   precision |   recall |   f1-score |    support |
|:-------------|------------:|---------:|-----------:|-----------:|
| 1            |    0.702703 | 0.675325 |   0.688742 | 231        |
| 2            |    0.648352 | 0.708    |   0.676864 | 250        |
| 3            |    0.375    | 0.2      |   0.26087  |  30        |
| accuracy     |    0.663405 | 0.663405 |   0.663405 |   0.663405 |
| macro avg    |    0.575351 | 0.527775 |   0.542159 | 511        |
| weighted avg |    0.656873 | 0.663405 |   0.657811 | 511        |

Looking at precision, the model performed best on the lower price categories, with a precision of 0.703 for price category 1 and 0.648 for price category 2. However, the model performed much worse on price category 3, with a precision of only 0.375 and a recall of 0.200. This suggests that the model struggles to correctly identify higher-priced establishments.

Overall, I do not think this model is very strong yet. While it performs reasonably well on the more common lower-price categories, it struggles with the less common higher-price category. This is likely because the dataset is imbalanced, with many more restaurants in price categories 1 and 2 than in category 3. As a result, the model has fewer examples to learn from for higher-priced restaurants.

Another limitation is that I did not apply much preprocessing to the numerical columns. For example, num_of_reviews has a very large range, and applying a transformation such as a log transformation could help reduce the effect of extreme values. To improve the model, I would experiment with additional preprocessing, tune the RandomForestClassifier hyperparameters, and possibly use class balancing techniques to help the model perform better on the less common price categories.

# Final Model

For my final model, I added two transformed features: **log_num_of_reviews** and **log_distance_to_nearest_beach_km**. These were created by applying a log transformation to **num_of_reviews** and **distance_to_nearest_beach_km**.

I chose to add these features because both original columns had a very large range of values. For example, some restaurants may have only a few reviews, while others may have thousands. Similarly, some restaurants are very close to a beach, while others are much farther away. In both cases, the difference between small values may be more meaningful than the difference between very large values. For example, the difference between 5 and 50 reviews may say more about a restaurant’s visibility than the difference between 1,005 and 1,050 reviews. A log transformation helps reduce the influence of extreme values while still preserving the general relationship between the variable and the target.

From the perspective of the data-generating process, I believe these transformations are useful because restaurant price categories are unlikely to increase linearly with review count or beach distance. A restaurant having more reviews may indicate popularity, tourist visibility, or a more established business, but the effect likely levels off after a certain point. Similarly, distance from the beach may matter more when comparing restaurants very close to the beach versus farther inland, rather than treating every additional kilometer equally.

Additionally, I applied a **StandardScaler** to **avg_rating**. Since most restaurant ratings were concentrated within a relatively high range, scaling this feature allows the model to compare ratings based on how high or low they are relative to the rest of the dataset, rather than only using the original 1–5 rating scale.

I also added a **broad_category** feature, which I created earlier when aggregating the original **category** column. Instead of using many highly specific categories, this feature groups establishments into more general types, such as **Restaurant**, **Dessert**, **Beverages**, **Bar / Nightlife**, **Small Shops**, and **Other**.

I believed this feature would be useful because the type of establishment is likely related to its price category. For example, dessert shops, cafes, and beverage establishments may be more likely to fall into lower price categories, while full-service restaurants may have a wider range of prices. By simplifying the original category values into broader groups, the model can learn general patterns between establishment type and Google price category without being overwhelmed by too many specific category labels.

This is useful because small differences in rating may still be meaningful. For example, the difference between a 4.2 and a 4.7 rating could reflect an important difference in customer perception, even though both are technically high ratings.

For my final modeling algorithm, I continued using a **RandomForestClassifier**. I chose this model because it can capture nonlinear relationships between features and price category, and it can also handle interactions between variables such as island, restaurant category, review count, and operating hours.

To improve the model, I used **GridSearchCV** to tune the hyperparameters. In particular, I tuned **max_depth** and **n_estimators**. The best-performing hyperparameters were:
max_depth = 20
n_estimators = 200

Here is the classification report:


|              |   precision |   recall |   f1-score |    support |
|:-------------|------------:|---------:|-----------:|-----------:|
| 1            |    0.733333 | 0.666667 |   0.698413 | 231        |
| 2            |    0.6609   | 0.764    |   0.70872  | 250        |
| 3            |    0.416667 | 0.166667 |   0.238095 |  30        |
| accuracy     |    0.684932 | 0.684932 |   0.684932 |   0.684932 |
| macro avg    |    0.603633 | 0.532444 |   0.548409 | 511        |
| weighted avg |    0.679305 | 0.684932 |   0.676431 | 511        |

The final model improved over the baseline model because the precision increased for each price category. This means that when the final model predicted a certain price category, it was more often correct compared to the baseline model. This is especially important for my project because the target variable is imbalanced, with fewer examples of higher-priced restaurants. Since precision measures the reliability of the model’s predictions, the increase in precision suggests that the final model is better at making accurate price category predictions across the different classes.

# Fairness Analysis

For my fairness analysis, I chose to compare model performance between restaurants with many reviews and restaurants with fewer reviews. I defined the two groups using **num_of_reviews** because restaurants with more reviews are likely more popular, more visible on Google, and may have more consistent information available. In contrast, restaurants with fewer reviews may be less represented or have less reliable information, which could affect the model’s performance.

**Group X:** Restaurants with **250 or more reviews**  
**Group Y:** Restaurants with **fewer than 250 reviews**

**Evaluation Metric:** Precision

I chose precision because my model predicts restaurant price categories, and I want to measure how reliable the model’s predictions are for each group. Since the target variable is imbalanced, accuracy alone may be misleading. Precision tells us, among the restaurants predicted to be in a certain price category, how often the model was actually correct.

**Null Hypothesis:**  
The model is fair. The precision of the model for restaurants with 250 or more reviews is the same as the precision of the model for restaurants with fewer than 250 reviews, and any observed difference is due to random chance.

**Alternative Hypothesis:**  
The model is not fair. The precision of the model is different for restaurants with 250 or more reviews compared to restaurants with fewer than 250 reviews.

**Test Statistic:**  
The absolute difference in precision between the two groups.

**Significance Level:**  
I used a 5% significance level, meaning that I reject the null hypothesis if the p-value is less than 0.05.

The resulting p-value was **0.277**. Since the p-value is **greater than** 0.05, I **fail to reject** the null hypothesis.

This means that there **is not** enough evidence to conclude that my model performs differently for restaurants with 250 or more reviews compared to restaurants with fewer than 250 reviews.


