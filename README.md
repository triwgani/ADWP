# Analyzing Data with Python
In today’s digital era, e-commerce has become a thriving industry, generating vast amounts of data that hold valuable insights for businesses. To unlock the potential of this data and gain a competitive edge, organizations are turning to Python, a powerful programming language with extensive data analysis capabilities. In this article, we will explore the fascinating world of analyzing e-commerce data with Python through a case study from an e-commerce firm. This case study is part#2 of the My Skill Data Analysis Bootcamp Case Project. From data collection and cleaning to exploratory analysis and visualization, we will delve into various techniques and tools that empower businesses to extract meaningful insights and make data-driven decisions.

![](pict01.png)

## Introduction

A join meeting was held in a company, and in order to follow up the meeting, a data analyst team has been assigned to address some issues the company is  currently working on. The data analyst team utilized the python to analyse some given data and draw conclusion from it.

## Dataset
The native dataset used inthis analysis is from Kaggle – Pakistan Largest E-Commerce Dataset with several with several modification. The listed price has been converted from Rupee to Rupiahs with currency rate of 1 Rupee = IDR 58. We can find the native datasets form [Kaggle](https://www.kaggle.com/datasets/zusmani/pakistans-largest-ecommerce-dataset). The datasets consist of 4 tables:
1. The Order Detail table (order_detail.csv)
2. Payment Detail table (payment_detail.csv)
3. Customer Detail table (customer_detail.csv)
4. Stock Keeping Unit table (sku_detail.csv)

```Sh
# Importing library - 1
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
from pandas.tseries.offsets import BDay

# Importing data sources to Google Colab -2
path_od = "https://raw.githubusercontent.com/dataskillsboost/myskill/main/order_detail.csv"
path_pd = "https://raw.githubusercontent.com/dataskillsboost/myskill/main/payment_detail.csv"
path_cd = "https://raw.githubusercontent.com/dataskillsboost/myskill/main/customer_detail.csv"
path_sd = "https://raw.githubusercontent.com/dataskillsboost/myskill/main/sku_detail.csv"
df_od = pd.read_csv(path_od)
df_pd = pd.read_csv(path_pd)
df_cd = pd.read_csv(path_cd)
df_sd = pd.read_csv(path_sd)
```
Exploring the Datasets:
```Sh
# Displaying the first 10 rows of the datasets -3A
df_od.head(10)
```
![](pict02.png)

```Sh
# Displaying the first 10 rows of the datasets -3B
df_pd.head(10)
df_cd.head(10)
df_sd.head(10)
```
![](pict03.png)

We also need to analyze the connection between the 4 tables in order to make further analysis and synthesize insights. Therefore, we need to create possible joins them.

```Sh
#Running SQL in Colab
from sqlite3 import connect

# Creating connection with sqlite
conn = connect(':memory:')

# dumping dataframe into a table in sqlite
df_od.to_sql('order_detail',conn, index=False, if_exists='replace')
df_pd.to_sql('payment_detail', conn, index=False, if_exists='replace')
df_sd.to_sql('sku_detail', conn, index=False, if_exists='replace')
df_cd.to_sql('customer_detail', conn, index=False, if_exists='replace')
df_od.to_sql
pd.read_sql('select * from sku_detail',conn)
df_sd.to_sql('sku_detail', conn, index=True, if_exists='replace')
pd.read_sql('select * from sku_detail', conn)
df_sd.to_sql('sku_detail', conn, index=False, if_exists='replace')
pd.read_sql('select * from sku_detail', conn).head()

# Joining datasets using Query SQL into a table
df = pd.read_sql("""
SELECT
    order_detail.*,
    payment_detail.payment_method,
    sku_detail.sku_name,
    sku_detail.base_price,
    sku_detail.cogs,
    sku_detail.category,
    customer_detail.registered_date
FROM order_detail
LEFT JOIN payment_detail
    on payment_detail.id = order_detail.payment_id
LEFT JOIN sku_detail
    on sku_detail.id = order_detail.sku_id
LEFT JOIN customer_detail
    on customer_detail.id = order_detail.customer_id
""",conn)
```

Displaying the first 5 rows of the joined datasets
```Sh
df.head(5)
```
![](pict04.png)

## Problem and Analysis
>### Case-1
> At the end of this year, the company will give prizes to customers who win the Year-End Festival competition. The Marketing Team needs help to determine the estimated prize that will be given to the winner of the competition later. These prizes will be taken from the TOP 5 Products from the Mobiles & Tablets Category during 2022, with the highest total sales quantity (valid = 1).

```Sh
#Storing the data in Pandas Data Frame
data1 = pd.DataFrame(\
                     #Filtering data with is_valid =1                     df[(df['is_valid']==1) &\
                        #Filtering data with category of Mobiles and Tablets
                        (df['category']=='Mobiles & Tablets') &\
                        #Filtering data for transaction during 2022
                        ((df['order_date'] >= '2022-01-01') & (df['order_date'] <= '2022-12-31'))]\
                     #Grouping the data
                     .groupby(by=["sku_name"])["qty_ordered"]\
                     #Summing the the quantity order based on the corresponding sku_name
                     .sum()\
                     #Sorting the data in descending way
                     .sort_values(ascending=False)\
                     #Selecting the top 5 product with the most quantity number
                     .head(5)\
                     #resetting the header
                     .reset_index(name='qty_2022'))
data1
```

![](pict05.png)

Based on the result, the TOP 5 Products from the Mobiles & Tablets Category during 2022, with the highest total  sales quantity (valid = 1) are shown above. The Marketing Team can determine the estimated prize that will be  given to the winner of the competition based on the provided solution.

>### Case-2
> Following up on the joint meeting of the Werehouse Team and Marketing Team, the Team found that there was still a lot of product stock in the Beauty & Grooming Category at the end of 2022. The Team asked for help to check the sales data for this category for 2021 in terms of sales quantity. Our provisional estimate is that there has been a decrease in sales quantity in 2022 compared to 2021. (Please also display data for the 15 categories).

```Sh
#Storing the data into Pandas DataFrame
    data2 = pd.DataFrame(\
    #Filtering Data with is_valid=1
    df[(df['is_valid']==1) &\
    #Filtering data for transaction during 2021
    ((df['order_date'] >= '2021-01-01') & (df['order_date'] <= '2021-12-31'))]\
    #grouping the data
    .groupby(by=["category"])["qty_ordered"]\
    #Summing the quantity
    .sum()\
    #Sorting the data
    .sort_values(ascending=False)\
    #Resetting header
    .reset_index(name='qty_2021'))
data2

#Storing data into Pandas DataFrame
    data3 = pd.DataFrame(\
    #Filtering Data with is_valid=1
    df[(df['is_valid']==1) &\
    #Filtering data for transaction during 2022
    ((df['order_date'] >= '2022-01-01') & (df['order_date'] <= '2022-12-31'))]\
    #grouping data
    .groupby(by=["category"])["qty_ordered"]\
    #summing the data
    .sum()\
    #Sorting the data
    .sort_values(ascending=False)\
    #Resetting header
    .reset_index(name='qty_2022'))
data3

data4 = pd.merge(data2,
    data3,
    left_on='category',
    right_on='category’)
    data4
    #Growth = Last Year – Previous Year
    data4['qty_growth'] = data4['qty_2022'] - data4['qty_2021']
data4
```

![](pict06.png)

The above syntax results the data of 15 Product categories in which some of them experienced decrement in Sales Quantity, while others experienced the other direction. Based on the result, Soghaat experienced the biggest decrement (most negative growth) with -5032 Sales Quantity, while the Superstore experienced the biggest increment (most positive growth) with 22631 Sales Quantity.

>### Case-3
> If there is indeed a decrease in the quantity of sales in the Beauty & Grooming category, the Team asked for assistance in providing data on the TOP 20 product names that have experienced the highest decline in 2022 when compared to 2021. We will use this as material for discussion at the next meeting.

```Sh
data5 = pd.DataFrame(\
    df[(df['is_valid']==1) &\
    (df['category']=='Beauty & Grooming') &\
    ((df['order_date'] >= '2021-01-01') & (df['order_date'] <= '2021-12-31'))]\
    groupby(by=["sku_name"])["qty_ordered"]\
    sum()\
    sort_values(ascending=False)\
    reset_index(name='qty_bg_2021'))
data5
data6 = pd.DataFrame(\
    df[(df['is_valid']==1) &\
    (df['category']=='Beauty & Grooming') &\
    ((df['order_date'] >= '2022-01-01') & (df['order_date'] <= '2022-12-31'))]\
    groupby(by=["sku_name"])["qty_ordered"]\
    sum()\
    sort_values(ascending=False)\
    reset_index(name='qty_bg_2022'))
data6
data7 = data5.merge(data6, left_on = 'sku_name', right_on = 'sku_name')
    data7['qty_bg_growth']=data7['qty_bg_2022']- data7['qty_bg_2021']
    data7.sort_values(by=['qty_bg_growth'],ascending=True, inplace=True)
    data7 = data7.head(20)
data7
```

![](pict07.png)

The above solution results the data of 20 Product name based on Category of Beauty and Grooming. All of products in the category experienced decrement in Sales Quantity. Based on the result, kcc_krone deal experienced the biggest decrement with -1135 Sales Quantity, followed by kcc_glamour deal with -360, and kcc_Buy 2 Frey Air Freshener* with -266 in the third place. The others are depicted in the table.

