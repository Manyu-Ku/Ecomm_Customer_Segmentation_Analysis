# Ecommerce Customer Segmentation Analysis

![ec](https://github.com/Manyu-Ku/Ecomm_Customer_Segmentation_Analysis/assets/122411152/3fad0697-4f6d-4ed3-839e-1f74101072eb)

Photo by <a href="https://unsplash.com/@olloweb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Agence Olloweb</a> on <a href="https://unsplash.com/photos/qfp4-Ud6Fyg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

> Data Source: [kaggle_datasets](https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-cosmetics-shop?datasetId=426888&sortBy=voteCount)
>
> The provided dataset encompasses behavior data spanning five months (Oct 2019 - Feb 2020) from a medium-sized cosmetics online store. 

## Table of Contents
- [1. Business Problem](#1-business-problem)
- [2. Objectives](#2-objectives)
- [3. Methodology](#3-methodology)
- [4. Data Analysis](#4-data-analysis)
  - [4-1 Data Preprocessing](#4-1-data-preprocessing)
  - [4-2 E-commerce Performence Exploration](#4-2-e-commerce-performence-exploration)
  - [4-3 RFM Calculation](#4-3-rfm-calculation-1)
  - [4-4 KMeans Clustering using RFM](#4-4-kmeans-clustering-using-rfm)
  - [4-5 Random Sampling and Pattern Observation for Clustering Validation](#4-5-random-sampling-and-pattern-observation-for-clustering-validation)
  - [4-6 Insights](#4-6-insights)
- [5. Recommendations](#5-recommendations)
- [6. References](#6-references)

## 1. Business Problem:
The business is facing a significant challenge in driving sales growth and improving the conversion rate. To address this pain point, the objective is to build personalized marketing campaigns based on different types of customers, determined by their behaviors. By segmenting customers and understanding their unique characteristics, targeted strategies can be implemented to enhance sales performance and increase the conversion rate.

## 2. Objectives:
This analysis aims to achieve the following objectives:
1. Assess the overall performance and trends of the business.
2. Segment customers based on their historical records.
3. Identify the patterns and characteristics of each customer segment.
4. Determine the peak traffic periods to inform future marketing strategies.

## 3. Methodology:
The analysis in this report was conducted using Jupyter Notebook and implemented in Python.
- Libraries used: `Pandas`, `NumPy`, `Matplotlib`, `Seaborn`, `Scikit-learn`
- Techniques/Models applied: `Exploratory Data Analysis`, `KMeans Clustering`, `RFM Model`, `Data Visualization`

## 4. Data Analysis
The following outline provides a concise summary of the analysis, highlighting the essential steps and key findings. To view the complete code, please click [here for ipynb file](https://github.com/Manyu-Ku/Ecomm_Customer_Segmentation_Analysis/blob/main/RFM%20Segmentation.ipynb).

### 4-1 Data Preprocessing
- Import libraries
- Read and concatenate datasets of 5 months
- View the first 5 & the last 5 rows of the dataset
- Check the size of the data and the data types of all columns
- Convert 'event_time' to DateTime format
- Check & handle NA values
- Dropping irrelevant columns
- Explore numeric variable 'price' & fix the abnormal values
- View the unique values count of all columns
- Create a sub dataset df_sales in order to calculate sales easily

### 4-2 E-commerce Performence Exploration
- Find out the traffic and sales trend
  
  <img width="702" alt="trend" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/b1762441-c887-4b7c-b0a1-f1b0abfc3f3b">

- Find out the monthly traffic, sales and conversion rate
  
  <img width="740" alt="monthly" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/5227cde7-ffe9-490e-b9a2-90b8f27b6bbc">

> :point_right:
> In February, all key metrics including traffic, sales, and conversion rate declined compared to the previous month. These metrics have shown no significant increases over the past five months, except for a seasonal peak in November. This serves as a warning signal.

- Find out the peak time of the traffic
  
  <img width="445" alt="peak" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/42047f2a-e4aa-4e9b-a122-9eaf4e9c1bb1">

> :point_right:
> The heatmap effectively addressed objective 4 by identifying the peak traffic periods, providing valuable information for informing future marketing strategies.

- Calculate cart abandonment rate (incompleted transactions/purchases)
  
  <img width="298" alt="ab" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/0f0315a5-6a30-439c-8f4e-d72fe32a8357">

- Calculate the average order value
  
  <img width="220" alt="avo" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/07e6b52f-1647-4b6f-a69d-c61efec3aeb8">

### 4-3 RFM Calculation [^1]
> The RFM model is based on three quantitative factors:
> 
> - **Recency**: How recently a customer has made a purchase
> - **Frequency**: How often a customer makes a purchase
> - **Monetary value**: How much money a customer spends on purchases
>
> [^1]: Investopedia. (2022, November 19). RFM (Recency, Frequency, Monetary Value). Retrieved from https://www.investopedia.com/terms/r/rfm-recency-frequency-monetary-value.asp

- Calculate 'recency' by month: set 2020 Feb as the reference month, label the user's most recent purchase month as follows: if the user's most recent purchase month is February, it is labeled as '0', and if it is January, it is labeled as '1'
- Calculate 'frequency' by counting the total number of sessions a user has had in the span of 5 months
- Calculate 'monetary' value by summing the total amount a user has spent in the span of 5 months
- Remove outliers & plot the distribution of RFM

  <img width="740" alt="hist" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/c9c354f8-fc64-4ba0-8c0e-ccc56795b6b5">

### 4-4 KMeans Clustering using RFM
- Standardize RFM data
- Determine the optimal cluster size using the elbow method, and adjust if necessary to align with the project's objectives.
- Apply the KMeans clustering algorithm and generate a scatter plot to visualize the results

  <img width="304" alt="cluster" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/14306c48-dc88-4c49-bd7e-f182cef33c18">

- Find out the mean values of each cluster

  <img width="199" alt="mean" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/9fc5bebd-8fa0-493c-aa70-9aa6401ae840">
  
- Renaming clusters according to their specific behaviors and features as follow:
  
 |Cluster No.|Features|Label as|
 |---|---|---|
 |0|customers with relatively recent activity, moderate frequency, and high monetary value|*Potential High-Value Customer*|
 |1|customers who have made recent purchases with low frequency and low monetary value|*New Customers*|
 |2|customers who made a purchase in the past but are now inactive|*Dormant Customers*|
 |3|customers who have shopped recently and exhibit high frequency and high monetary value|*Loyal Customers*|

- Find out the proportion of each cluster and plot the clusters' mean values of RFM

  <img width="668" alt="pie" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/48263eb4-90c7-428f-82b2-10fed5775206">

  > :point_right:
  > The proportions of the two profitable segments, `Potential High-Value Customers` and `Loyal Customers`, are relatively low. On the other hand, the proportion of `Dormant Customers` is relatively high. The primary objective of the business is to decrease the proportion of `Dormant Customers`.

### 4-5 Random Sampling and Pattern Observation for Clustering Validation
- Combine the cluster data with the original dataframe to analyze and observe distinct trends among the clusters
- Find out the cart abandonment rate by cluster
  
  <img width="353" alt="cartab" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/ef344cc7-f032-449c-8359-c3041bea8c04">

- Calculate average order value by cluster

  <img width="437" alt="avoc" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/f32f7419-6cf7-4c75-bbf8-9c2a04ba2e10">

- Plot the distribution of active months by cluster

  <img width="745" alt="active" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/7abd90c3-2967-4cf8-81aa-af6b3bd12490">

- Take random 30 samples from each cluster to observe their behavior records

  <img width="744" alt="new" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/cdbed775-6e0f-40c7-9709-63280ca73175">

  <img width="746" alt="loyal" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/00cd1d1c-fc74-438d-99d8-d9be964db515">

  <img width="748" alt="dormant" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/98d4154d-6653-4952-80fd-73cb9358cd16">

  <img width="736" alt="potential" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/e38fcd73-840b-4e2a-8519-a81b8657cadf">

### 4-6 Insights
1. Dormant customers represent approximately one-third of the total customer base. Upon analyzing the sample records and observing a mean frequency of 1.06, it is evident that most dormant customers are one-time purchasers. This highlights a significant challenge for the business in terms of customer retention.

2. Loyal customers exhibit a high purchase frequency and generate substantial sales, making them a valuable segment for any business. However, it is noteworthy that their cart abandonment rate is the highest among all clusters. This could be attributed to a higher number of products being added to the cart compared to other clusters.

3. New customers account for nearly half of all customers, which is a positive sign if the business is performing well. However, based on the observed traffic and sales trends, it is evident that the business struggles to retain a significant portion of new customers.

4. Potential high-value customers represent another profitable and stable cluster that requires attention. They exhibit the lowest cart abandonment rate and the highest average order values. It is essential to monitor the frequency of this cluster over a longer period to determine whether their shopping habits remain consistent or if their interest starts to decline.

## 5. Recommendations
Based on the insights, here are some tailored recommendations:

1. Conduct a customer survey: It is recommended to conduct a customer survey targeting dormant customers. Offer incentives such as free samples or discounts to encourage their participation. Through the survey, gather insights into their reasons for not shopping with the business anymore. This information can help identify areas of improvement and develop targeted strategies to win back their engagement.

2. Re-engage new customers: Send limited-time discount offers or exclusive promotions to new customers who have been inactive for a certain period, such as 20 days. By providing enticing incentives, the business can encourage these customers to make repeat purchases and increase their level of engagement.

3. Nurture potential high-value customers: Pay special attention to potential high-value customers who show promising purchase behavior. Implement personalized marketing strategies, such as tailored product recommendations or exclusive perks, to maintain their interest and loyalty. Monitor their shopping habits over time to ensure they remain engaged and satisfied with their experience.

By implementing these recommendations, the business can actively address customer retention issues, re-engage dormant customers, cultivate loyalty among new customers, and maximize the potential of high-value customers.

## 6. References
17 eCommerce metrics that you should be tracking. (2022, August 9). Wix ECommerce Blog[^2]

RFM Segmentation | RFM Analysis, Model, Marketing & Software. (n.d.). Optimove[^3]

Jianlizhou. (2020, December 6). Customer segmentation by RFM model and K-means. Kaggle[^4]

[^2]: 17 eCommerce metrics that you should be tracking. (2022, August 9). Wix ECommerce Blog. https://www.wix.com/blog/ecommerce/2022/08/ecommerce-metrics?utm_source=google&utm_medium=cpc&utm_campaign=19554094039

[^3]: RFM Segmentation | RFM Analysis, Model, Marketing & Software. (n.d.). Optimove. https://www.optimove.com/resources/learning-center/rfm-segmentation

[^4]: Jianlizhou. (2020, December 6). Customer segmentation by RFM model and K-means. Kaggle. https://www.kaggle.com/code/jianlizhou/customer-segmentation-by-rfm-model-and-k-means 
