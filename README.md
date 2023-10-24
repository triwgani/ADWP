# Analyzing Data with Python
In today’s digital era, e-commerce has become a thriving industry, generating vast amounts of data that hold valuable insights for businesses. To unlock the potential of this data and gain a competitive edge, organizations are turning to Python, a powerful programming language with extensive data analysis capabilities. In this article, we will explore the fascinating world of analyzing e-commerce data with Python through a case study from an e-commerce firm. This case study is part#2 of the My Skill Data Analysis Bootcamp Case Project. From data collection and cleaning to exploratory analysis and visualization, we will delve into various techniques and tools that empower businesses to extract meaningful insights and make data-driven decisions.

## Introduction
![](pict01.png)

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

