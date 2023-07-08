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
- Import libraries<details>
  <summary>click to see code</summary>
 
  ```py
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  from mpl_toolkits.mplot3d import Axes3D
  import seaborn as sns
  from sklearn.cluster import KMeans
  from sklearn.preprocessing import StandardScaler
  ```
 </details>

- Read and concatenate datasets of 5 months<details>
  <summary>click to see code</summary>
  
  ```py
  df1=pd.read_csv('dataset/archive/2019-Oct.csv')
  df2=pd.read_csv('dataset/archive/2019-Nov.csv')
  df3=pd.read_csv('dataset/archive/2019-Dec.csv')
  df4=pd.read_csv('dataset/archive/2020-Jan.csv')
  df5=pd.read_csv('dataset/archive/2020-Feb.csv')
  df=pd.concat([df1,df2,df3,df4,df5], axis=0, ignore_index=True)
  ```
</details>

- View the first 5 & the last 5 rows of the dataset<details>
  <summary>click to see code</summary>
  
  ```py
  df.head()
  df.tail()
  ```
</details>

  🪄 First 5 rows:   
 
<img width="726" alt="head" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/42320f8e-948c-4441-9878-5e35cc678c58">

  🪄 Last 5 rows:

<img width="746" alt="tail" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/3fb748d6-f502-4e69-a482-90debe69be9d">

- Check the size of the data and the data types of all columns<details>
  <summary>click to see code</summary>
  
  ```py
  df.shape
  df.info()
  ```
</details>

  |Shape(rows, columns)|Data types of columns|
  |---|---|
  |<img width="80" alt="shape" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/7546babb-0e9d-4dcd-9cb5-78ea15d53b7f">|<img width="275" alt="info" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/e4198e78-ea39-4f10-aa6a-76a672d2e2f6">|

- Convert 'event_time' to DateTime format<details>
  <summary>click to see code</summary>
  
  ```py
  df['event_time'] = pd.to_datetime(df['event_time'].str.strip(to_strip=' UTC'), infer_datetime_format=True)
  ```
</details>


- Check NA values<details>
  <summary>click to see code</summary>
  
  ```py
  na_count = df.isna().sum()
  na_proportion = df.isna().sum()/len(df)
  pd.DataFrame({'NA Count': na_count, 'NA Proportion': na_proportion})
  ```
</details>

  🪄 NA values:

<img width="203" alt="na" src="https://github.com/Manyu-Ku/8_Week_SQL_Challenge/assets/122411152/f1f62bbe-d954-4336-925d-deb0721f405d">

> Given that the primary focus of the project is customer segmentation, we will exclude some irrelevant columns, including 'category_code' and 'brand'. Regarding the 'user_session' column, as the NA ratio is low, we will retain the data as is to maintain its integrity.

- Dropping irrelevant columns<details>
  <summary>click to see code</summary>
  
  ```py
  df = df.drop(columns=['product_id', 'category_id', 'category_code', 'brand'])
  ```
</details>

- numeric variable explore<details>
  <summary>click to see code</summary>
  
  ```py
  df = df.drop(columns=['product_id', 'category_id', 'category_code', 'brand'])
  ```
</details>
