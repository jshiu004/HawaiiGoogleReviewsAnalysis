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

<iframe
  src="assets/avg_rating_violin.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This distribution shows that restaurant ratings are heavily concentrated above 3.5, with relatively few establishments receiving lower ratings. The median rating is 4.5, suggesting that most food establishments in the dataset are rated highly by Google reviewers.

However, this should be interpreted carefully because Google ratings tend to be skewed toward higher values. A high rating may indicate customer satisfaction, but it may also reflect review bias, since people who leave reviews may not represent all customers equally.

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Final Model

# Fairness Analysis


