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
    | `SKU`   | Stock Keeping Unit a unique code used to identify a specific product.                           |
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
3. Data Cleaning & Formatting
4. Univariate Analysis
5. Bivariate/Multivariate Analysis
6. Outlier Detection
7. Data Quality Summary


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
- This summary describes statistical distribution of 15 numerical columns and 9 categorical columns.
-   All columns have no missing values, with count of 100, the numerical columns contain no negative values.
- The `Revenue generated` column has a high standard deviation, indicating that some products have very low while others have very high sales
- The `Defect rates` column has the lowest standard deviation, suggesting that only a small portion of outputs failed to meet quality standards.
- There are three product types, with skincare being the most commonly sold.
- Road transportation is the primary mode used to deliver products to distribution centers or suppliers.


    ![IMAGE 3: SUMMARY STATISTIC 1](3_summary_stat1.png)

    ![IMAGE 4: SUMMARY STATISTIC 2](4_summary_stat2.png)

---
### 3. Data Cleaning & Formatting

```python
# Format all floating-point numbers in the table to display with 2 decimal places
pd.set_option('display.float_format', lambda x: '%.2f' % x)

# Rename columns for better clarity and convenience during analysis
df.rename(columns={'Lead times': 'Supplier lead time', 'Lead time': 'Material lead time'}, inplace=True)

# Double-check column renaming and decimal formatting
df.describe()

# Save cleaned data from Python to a CSV
df.to_csv('supplychain_cleaned_data.csv', index=False)
```

- Below snapshot for cleaned numeric columns :
![IMAGE 5: SUMMARY STATISTIC 2](5_summary_stat_cleaned.png)

---
### 4. Univariate Analysis

This section will examine the distribution of individual variables to understand their spread and frequency which helps to identify skewness, outliers and potential data transformation needs.

-   Numeric variables are visualized by histograms and skewness analysis to understand their distribution.
- Categorical variables are visualized by bar charts to determine the most frequent categories.
---
```python
# Histogram and skewness for numerical variable

from scipy.stats import skew

# Calculate skewness of Revenue generated
skew_revenue = skew(df["Revenue generated"])

# Plot distribution with KDE (smoothed line)
plt.figure(figsize=(8, 5))
sns.histplot(df["Revenue generated"], kde=True, bins=20, color='darksalmon')
plt.title(f"Distribution of revenue (Skewness={skew_revenue:.2f})")
plt.xlabel("Revenue Generated")
plt.ylabel("Frequency")
plt.show()

# Print skewness
print(f"Skewness of revenue generated: {skew_revenue:.2f}")
```
![IMAGE 6: HISTOGRAM SKEW](6_histo_skew_revenue.png)
- Based on the distribution of `Revenue Generated`, the data slightly skew to the left since skewness is -0.17 and very close to 0 then it almost normally distributed.
- The histogram shows bimodal distribution with the first peak around 2,500 Rupee and the second peak around 8,000 Rupee
- The `Revenue Generated` data set shows 2 distinct revenue groups where there are low to mid revenue transactions and high value transactions.
---

![IMAGE 7: HISTOGRAM SKEW](7_histo_skew_shippingtimes.png)
- The distribution of `Shipping Times` is left-skewed with skewness of -0.28 which is close to normal distribution.
- According to the histogram, the most common shipping time from distribution centre to customer is aroud 8 days while the shortest is approxiamately 2 to 4 days.
- Deliveries about 2-4 days indicate the shipments to nearby locations and reflect operational efficiency in certain regions.
- Since the most shipping time around 8 days, it considered as slow potentially, lead to customer dissatisfaction. Collecting and analyzing customer feedback can help to identify areas for service improvement.
---
```python
# Bar chart for categorical variable

# Count the frequency of each type in Product Type
count_type = df['Product type'].value_counts().reset_index()

# Rename columns for clarity
count_type.columns = ['Product Type', 'Count']

# Create figure and axes for the chart
fig, ax = plt.subplots(figsize=(8, 5))

# Create a bar chart to show the count of each product type
sns.barplot(
    x=count_type['Product Type'],
    y=count_type['Count'],
    hue=count_type['Product Type'],
    palette="Blues", ax=ax)

# Set labels and title on axes
ax.set_title("Bar Plot of Product Type")
ax.set_xlabel("Product Type") 
ax.set_ylabel("Count")

# Display the plot
plt.show()
```
![IMAGE 8: BAR CHART](8_bar_producttype.png)

- The company sells three types of products, with skincare having the highest number of SKUs, followed by haircare and cosmetics.
- This suggests skincare is the company's main product focus.

---

![IMAGE 9: BAR CHART](9_bar_demographic.png)

- Based on the bar plot, most SKUs are associated with an unknown customer demographic, followed by female, non-binary, and male customers.
- Products labeled with 'Unknown' in the customer demographics may indicate items that are used by any gender and potentially suitable for all age groups, including children.
---

### 5. Bivariate and Multivariate Analysis

#### Bivariate Analysis

To indentify the relationship between variables, three functions will be define, based on the types of varables: categorical and numerical with continous variables. 

Function will be define based on type of variable: 
 - Scatter plot, Pearson and Spearman correlation will be used to determine the relationship between continous numerical variables.
 - The Chi Square test will be used to check the relationship between categorical variables.
 - The ANOVA test will be used to determine relationship between categorical and numerical variables.


```python
# Import library
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from scipy.stats import pearsonr, spearmans, chi2_contingency
```

```python
# Bivariate analysis between numerical variables (Correlation & Scatter Plot)
def numerical_bivariate_analysis(df, var1, var2):
    print(f"Pearson Correlation: {pearsonr(df[var1], df[var2])}")
    print(f"Spearman Correlation: {spearmanr(df[var1], df[var2])}")

    plt.figure(figsize=(8, 5))
    sns.scatterplot(data=df, x=var1, y=var2)
    plt.title(f'Scatter Plot of {var1} vs {var2}')
    plt.show()

# Bivariate analysis between numerical & categorical variable (Boxplot & ANOVA)
def categorical_numerical_analysis(df, cat_var, num_var):
    plt.figure(figsize=(15,8))
    sns.boxplot(data=df, x=cat_var, y=num_var)
    plt.title(f'Boxplot of {num_var} by {cat_var}')
    plt.show()

# Bivariate analysis between categorical variables (Chi-square test)
def categorical_bivariate_analysis(df, var1, var2):
    contingency_table = pd.crosstab(df[var1], df[var2])
    chi2, p, dof, expected = chi2_contingency(contingency_table)
    print(f"Chi-square Test: Chi2={chi2}, p-value={p}")

    plt.figure(figsize=(8, 5))
    sns.heatmap(contingency_table, annot=True, cmap="Blues", fmt='d')
    plt.title(f'Heatmap of {var1} vs {var2}')
    plt.show()
```
---

`Production volumes` vs `Manufacturing lead time`

```python
numerical_bivariate_analysis(df,'Production volumes','Manufacturing lead time')
```
Output:

 ![IMAGE 10: BAR CHART](10_bivariate_num.png)

-   Both Pearson and Spearman correlation analyses and the p values greater than 0.05 show no statistically significant relationship between the variables. 
- The correlation coefficient are low and and the scatter plot shows no liner or monotonic trend and therefore there are no relationship between the variables.

Overall, a correlation anlysis was conducted to exploe the relationship between all numerical variables using scatter plot, Pearson and Spearman corelation tests.

Across all variables, no statistically significant relationship were found since the p-values exceeding 0.05. Then, all the numerical variables operate independently. As a result, other analytical methods may be use for appropiate  for identifying patterns in the data. 

---
`Shipping carriers` vs `NUmber of product sold`

```python
categorical_numerical_analysis(df,'Shipping carriers','Number of products sold')
```
![IMAGE 11: BAR CHART](11_bivariate_cat_num.png)

---
`Product type` vs `Customer demographics`

```python
categorical_bivariate_analysis(df,'Product type','Customer demographics')
```

![IMAGE 12: BAR CHART](12_bivariate_cat.png)

---
### 8. cleaned data
 -  Download Link for cleaned data: [*Supply Chain Cleaned Data*](https://github.com/azerinnan/draft_EDA_supplychain/blob/main/supplychain_cleaned_data.csv)