# Ecommerce Customer Segmentation Analysis

<img src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/9e26e724-a0cc-4ba6-a4a0-eb0b45332f91" width="500">

credit: <a href="http://www.freepik.com">Designed by sentavio / Freepik</a>

> Data Source: [kaggle_datasets](https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-cosmetics-shop?datasetId=426888&sortBy=voteCount)
>
> The provided dataset encompasses behavior data spanning five months (Oct 2019 - Feb 2020) from a medium-sized cosmetics online store. 

## Business Problem:
The business is facing a significant challenge in driving sales growth and improving the conversion rate. To address this pain point, the objective is to build personalized marketing campaigns based on different types of customers, determined by their behaviors. By segmenting customers and understanding their unique characteristics, targeted strategies can be implemented to enhance sales performance and increase the conversion rate.

## Objectives:
This analysis aims to achieve the following objectives:
1. Assess the overall performance and trends of the business.
2. Segment customers based on their historical records.
3. Identify the patterns and characteristics of each customer segment.
4. Determine the peak traffic periods to inform future marketing strategies.

## Methodology:
The analysis in this report was conducted using Jupyter Notebook and implemented in Python.
- Libraries used: `Pandas`, `NumPy`, `Matplotlib`, `Seaborn`, `Scikit-learn`
- Techniques/Models applied: `Exploratory Data Analysis`, `KMeans Clustering`, `RFM Model`, `Data Visualization`

## Data Analysis
The following outline highlights the essential steps of the analysis. Please note that some code may be omitted for brevity. To view the complete code, please click [here]().
### Data Preprocessing
- Import libraries
  ```py
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  from mpl_toolkits.mplot3d import Axes3D
  import seaborn as sns
  from sklearn.cluster import KMeans
  from sklearn.preprocessing import StandardScaler
  ```
  
- Read and concatenate datasets of 5 months
  ```py
  df1=pd.read_csv('dataset/archive/2019-Oct.csv')
  df2=pd.read_csv('dataset/archive/2019-Nov.csv')
  df3=pd.read_csv('dataset/archive/2019-Dec.csv')
  df4=pd.read_csv('dataset/archive/2020-Jan.csv')
  df5=pd.read_csv('dataset/archive/2020-Feb.csv')
  
  df=pd.concat([df1,df2,df3,df4,df5], axis=0, ignore_index=True)
  ```

- View the first 5 & the last 5 rows of the dataset
  ```py
  df.head()
  ```
  <img width="726" alt="head" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/42320f8e-948c-4441-9878-5e35cc678c58">
  
  ```py
  df.tail()
  ```
  <img width="746" alt="tail" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/3fb748d6-f502-4e69-a482-90debe69be9d">

- Check the size of the data and the data types of all columns
  ```py
  df.shape
  ```
  <img width="80" alt="shape" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/7546babb-0e9d-4dcd-9cb5-78ea15d53b7f">

  ```py
  df.info()
  ```
  <img width="275" alt="info" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/e4198e78-ea39-4f10-aa6a-76a672d2e2f6">

- Convert 'event_time' to DateTime format
  ```py
  df['event_time'] = pd.to_datetime(df['event_time'].str.strip(to_strip=' UTC'), infer_datetime_format=True)
  ```

- Check NA values
  ```py
  na_count = df.isna().sum()
  na_proportion = df.isna().sum()/len(df)
  pd.DataFrame({'NA Count': na_count, 'NA Proportion': na_proportion})
  ```
  <img width="203" alt="na" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/f1f62bbe-d954-4336-925d-deb0721f405d">

  > Given that the primary focus of the project is customer segmentation, we will exclude some irrelevant columns, including 'category_code' and 'brand'. Regarding the 'user_session' column, as the NA ratio is low, we will retain the data as is to maintain its integrity.

- Dropping irrelevant columns
  ```py
  df = df.drop(columns=['product_id', 'category_id', 'category_code', 'brand'])
  ```

- explore numeric variable 'price'
  ```py
  df.price.describe()
  ```
  <img width="180" alt="neg" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/b642c999-c1e3-4c48-8769-d583d989469b">

  ```py
  # check the negative values in 'price'
  df[df.price < 0]
  ```
  <img width="572" alt="negp" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/d9ee627c-b5e3-48b8-a351-414f234df23f">

  > Assuming that negative values correspond to the event type 'remove_from_cart', which has been proven incorrect, we will convert all negative values to positive to avoid confusion.
  ```py
  # convert all negative values to positive to avoid confusion.
  df.loc[df.price < 0, 'price'] = df.loc[df.price < 0, 'price'].abs()
  ```

- view the unique values count of all columns
  ```py
  df.nunique()
  ```
  <img width="156" alt="unique" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/e8086de0-d29f-460a-8c9a-b2256a189ad8">

- creat a sub dataset `df_sales` in order to calculate sales easily
  ```py
  df_sales = df[df.event_type == 'purchase']
  ```

### E-commerce Performence Exploration
- find out the traffic and sales trend
  ```py
  plt.style.use('seaborn-deep')
  fig, ax1 = plt.subplots(figsize=(14, 4))
  df.resample('D', on='event_time').user_id.nunique().plot(ax=ax1, color='royalblue')
  ax1.set_xlabel('')
  ax1.set_ylabel('visitors count', color='royalblue')

  ax2 = ax1.twinx()
  df_sales.resample('D', on='event_time').price.sum().plot(ax=ax2, color='lightcoral')
  ax2.set_ylabel('total sales', color='lightcoral')
  
  ax1.set_title('Total Visitors And Sales Trend')
  plt.show()
  ```
  <img width="702" alt="trend" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/b1762441-c887-4b7c-b0a1-f1b0abfc3f3b">

- find out the monthly traffic, sales and conversion rate
  ```py
  plt.figure(figsize=(14,4))
  plt.subplot(1,3,1)
  total_user=df.groupby(df.event_time.dt.to_period('M')).user_id.nunique()
  total_user.plot(kind='bar', color='royalblue')
  plt.xlabel('')
  plt.xticks(rotation=45)
  plt.title('Total Visitors By Month')

  plt.subplot(1,3,2)
  monthly_sales=df_sales.groupby(df.event_time.dt.to_period('M')).price.sum()
  monthly_sales.plot(kind='bar', color='lightcoral')
  plt.xlabel('')
  plt.xticks(rotation=45)
  plt.title('Sales By Month')

  plt.subplot(1,3,3)
  conver = df_sales.groupby(df.event_time.dt.to_period('M')).user_id.nunique()/total_user
  conver.plot(color='lightcoral')
  plt.xlabel('')
  plt.xticks(rotation=45)
  plt.title('Conversion Rate By Month')
  plt.tight_layout()
  plt.show()
  ```
  <img width="740" alt="monthly" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/5227cde7-ffe9-490e-b9a2-90b8f27b6bbc">

> **:pushpin:
> In February, all key metrics including traffic, sales, and conversion rate declined compared to the previous month. These metrics have shown no significant increases over the past five months, except for a seasonal peak in November. This serves as a warning signal.**

- find out the peak time of the traffic
  ```py
  plt.figure(figsize=(10,3))
  table = df.pivot_table(index=df.event_time.dt.weekday, columns=df.event_time.dt.hour, values='user_id', aggfunc=pd.Series.nunique)
  sns.heatmap(table, cmap='Blues')
  y=[0,1,2,3,4,5,6]
  labels=['Mon','Tue','Wed','Thu','Fri','Sat','Sun']
  plt.yticks(y, labels, rotation=45)
  plt.xlabel('Hour')
  plt.ylabel('')
  plt.title('Peak Time Of Traffic')
  plt.show()
  ```
  <img width="445" alt="peak" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/42047f2a-e4aa-4e9b-a122-9eaf4e9c1bb1">

- calculate cart abandonment rate (incompleted transactions/purchases)
  ```py
  purchase = df_sales.groupby(df.event_time.dt.to_period('M')).user_id.count()
  cart = df[df.event_type == 'cart'].groupby(df.event_time.dt.to_period('M')).user_id.count()
  ab = 1 -(purchase/cart)
  ab.plot(color='lightcoral')
  plt.xlabel('')
  plt.title('Cart Abandonment Rate')
  plt.show()
  ```
  <img width="298" alt="ab" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/0f0315a5-6a30-439c-8f4e-d72fe32a8357">

- calculate the average order value
  ```py
  aov = round(df_sales.price.sum()/df_sales.user_session.nunique(),2)
  print(f"The average order value is ${aov}.")
  ```
  <img width="220" alt="avo" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/07e6b52f-1647-4b6f-a69d-c61efec3aeb8">

