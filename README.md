# Exploratory Data Analysis (EDA) – Supply Chain Dataset (Makeup Products)

## Project Overview & Objective
Conduct a full exploratory data analysis (EDA) to understand data types, distributions, missing values, outliers, and relationships between variables in a supply chain dataset. This EDA will act as a foundation for future objective-specific projects.

In this project, we analyze supply chain data from a Fashion and Beauty startup, specifically focusing on the movement and performance of makeup products across different operational stages — from suppliers to customers.

## Data Source
- Dataset Name: Supply Chain Data
- Source: Kaggle
- Data Format: CSV
- Download Link: [*Supply Chain Data (Kaggle)*](https://www.kaggle.com/datasets/harshsingh2209/supply-chain-analysis/data)

    | Variable Name               | Description                                                                 |
    |----------------------------|-----------------------------------------------------------------------------|
    | `Product Type`               | The type of product represented in the supply chain data.                    |
    | `SKU (Stock Keeping Unit)`   | A unique code used to identify a specific product.                           |
    | `Price`                      | The selling price of the product.                                            |
    | `Availability`               | The current availability of the product (units in stock).                   |
    | `Number of Products Sold`    | The number of units sold during a specific time period.                      |
    | `Revenue Generated`          | The total revenue generated from product sales over a specific period.       |
    | `Customer Demographics`      | Information about customer characteristics, such as age, gender, and location.|
    | `Stock Levels`               | The number of units currently available in stock.                            |
    | `Lead Times`                 | The time required to order or receive products from suppliers.               |
    | `Order Quantities`           | The number of units ordered in a single purchase or shipment.                |
    | `Shipping Times`             | The duration required to ship products to customers.                         |
    | `Shipping Carriers`          | The companies or services used to deliver products.                          |
    | `Shipping Costs`             | Costs associated with shipping, including delivery and additional fees.      |
    | `Supplier Name`              | The name of the supplier or vendor providing the products or materials.      |
    | `Location`                   | The physical location of the warehouse or distribution center.               |
    | `Lead Time (Materials)`      | The time required to acquire raw materials from suppliers.                   |
    | `Production Volumes`         | The number of units produced during a specific time period.                  |
    | `Manufacturing Lead Time`    | The time taken to manufacture a product from start to finish.                |
    | `Manufacturing Costs`        | Expenses related to the production process, including materials and labor.   |
    | `Inspection Results`         | Outcomes of product or material quality inspections.                         |
    | `Defect Rates`               | The percentage or rate of defective products produced.                       |
    | `Transportation Modes`       | The mode of transportation used.                      |
    | `Routes`                     | The delivery paths taken to transport products from origin to destination.   |
    | `Costs`                      | Total costs associated with the supply chain, including production and logistics. |

## Workflow Steps
1. Import Libraries & Load Data
2. Initial Data Overview
3. Missing Value Analysis
4. Univariate Analysis
5. Bivariate/Multivariate Analysis
6. Outlier Detection
7. Data Quality Summary
8. Save Cleaned Dataset

## Exploratory Data Analysis (EDA)

### 1. Import Libraries & Load Data

```python
# Importing the necessary libraries for data analysis and visualization.

import pandas as pd
import numpy as np 
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
import plotly.graph_objects as go


# Load the dataset into a DataFrame using 'pandas'.

df = pd.read_csv("supply-chain-analysis/supply_chain_data.csv")
```
---
### 2. Initial Data Overview

Inspect the dataset to understand its structure, data type, and identify any missing value:

```python
# Check the number of rows and columns
df.shape
```

- The dataset contains **100 rows** and **24 columns**

```python
# Preview the top 5 records
df.head()
```
-   The first five rows provide a snapshot of the dataset structure, and all columns appear to be correctly typed.
- However, some currency-related data shows more than two decimal places and will be converted for consistency.   
    ![IMAGE 1: TOP 5 RECORDS](1_top5_records.png)

```python
# Check data type and null counts
df.info()
```
-   All data types are appropriate, and there are no missing values.
- However, the column names **'Lead times'** (receive product from suppliers) and **'Lead Time'** (for manufacturer receive raw materials) may cause confusion.
To improve clarity, both columns will be renamed

    <img src="2_data_info.png" width="40%"/> 
```python
# Summary of statistics
df.describe()

df.describe(include='object') ## Only categorical columns
```
- This summary describe statistically the 15 columns of numerical and 9 columns of categorical variables.
-   All columns have no missin values with count of 100 and no negative values in numeric columns.
- `Revenue generated` has high standard deviation shows some products have low and high sales.
- `Defect rates` is the lowest standard deviation shows small number outputs failed to meet quality standards.
- There are 3 type of products and skincare is the major type product sell
- Road transportation mode is the major transportation to deliver to distribution centre or supplier


    ![IMAGE 3: SUMMARY STATISTIC 1](3_summary_stat1.png)
    ![IMAGE 3: SUMMARY STATISTIC 2](4_summary_stat2.png)






### 3. Data Cleaning & Formatting