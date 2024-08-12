# Analyzing Online Retail Sales Data 

## Overview

This project performs an analysis of an online retail dataset to uncover insights into sales performance, identify top-selling products, and segment customers based on their purchasing behavior. The analysis includes data cleaning, exploratory data analysis, and visualization using Python libraries such as Pandas, Matplotlib, and Seaborn.


## Dataset

The dataset used for this analysis is the **Online Retail dataset** from the UCI Machine Learning Repository. It contains transactional data from a UK-based online retailer.

- **Source**: [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail)
  

## Installation

Ensure you have Python installed on your system. You will also need to install the required libraries. You can do this using pip:


pip install pandas matplotlib seaborn openpyxl


## Explanation of Code



### 1. Import Libraries

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

- **`pandas`**: A library for data manipulation and analysis.
- **`matplotlib.pyplot`**: A plotting library for creating static, animated, and interactive visualizations.
- **`seaborn`**: A statistical data visualization library based on Matplotlib, providing a high-level interface for drawing attractive and informative graphics.

### 2. Load the Dataset

```python
# Load the dataset
url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/00352/Online%20Retail.xlsx'
df = pd.read_excel(url, sheet_name='Online Retail')
```

- **`url`**: The URL where the dataset is located.
- **`pd.read_excel()`**: Reads the Excel file from the URL into a DataFrame named `df`.

### 3. Data Cleaning

```python
# Data Cleaning
df.dropna(subset=['CustomerID'], inplace=True)  # Remove rows with missing CustomerID
df['TotalAmount'] = df['UnitPrice'] * df['Quantity']  # Create a TotalAmount column
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])  # Convert InvoiceDate to datetime
```

- **`df.dropna(subset=['CustomerID'], inplace=True)`**: Removes rows where `CustomerID` is missing.
- **`df['TotalAmount'] = df['UnitPrice'] * df['Quantity']`**: Creates a new column `TotalAmount` by multiplying `UnitPrice` with `Quantity`.
- **`pd.to_datetime(df['InvoiceDate'])`**: Converts the `InvoiceDate` column to datetime format for easier manipulation.

### 4. Data Exploration

```python
# Data Exploration
print(df.describe())
print(df.info())
print(df.head())
```

- **`df.describe()`**: Provides statistical summaries of numerical columns.
- **`df.info()`**: Displays information about the DataFrame including data types and non-null counts.
- **`df.head()`**: Shows the first few rows of the DataFrame to give an overview of the data.

### 5. Sales Over Time

```python
# Sales Over Time
df.set_index('InvoiceDate', inplace=True)
monthly_sales = df['TotalAmount'].resample('M').sum()

plt.figure(figsize=(12, 6))
sns.lineplot(data=monthly_sales)
plt.title('Monthly Sales Over Time')
plt.xlabel('Date')
plt.ylabel('Total Sales')
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('images/monthly_sales.png')  # Save plot as image
plt.close()
```

- **`df.set_index('InvoiceDate', inplace=True)`**: Sets the `InvoiceDate` column as the index for the DataFrame.
- **`df['TotalAmount'].resample('M').sum()`**: Resamples the data to monthly frequency and sums the `TotalAmount` for each month.
- **`plt.figure(figsize=(12, 6))`**: Creates a new figure with the specified size.
- **`sns.lineplot(data=monthly_sales)`**: Plots a line graph of monthly sales.
- **`plt.title()`, `plt.xlabel()`, `plt.ylabel()`**: Sets the title and labels for the plot.
- **`plt.xticks(rotation=45)`**: Rotates x-axis labels for better readability.
- **`plt.tight_layout()`**: Adjusts the layout to fit the figure area.
- **`plt.savefig()`**: Saves the plot as an image file.
- **`plt.close()`**: Closes the figure to free up memory.

### 6. Top Products by Sales

```python
# Top Products by Sales
top_products = df.groupby('Description')['TotalAmount'].sum().sort_values(ascending=False).head(10)

plt.figure(figsize=(12, 6))
sns.barplot(x=top_products.index, y=top_products.values, palette='rainbow')
plt.title('Top 10 Products by Sales')
plt.xlabel('Product')
plt.ylabel('Total Sales')
plt.xticks(rotation=90)
plt.tight_layout()
plt.savefig('images/top_products.png')  # Save plot as image
plt.close()
```

- **`df.groupby('Description')['TotalAmount'].sum()`**: Groups the data by `Description` and calculates the total sales amount for each product.
- **`.sort_values(ascending=False)`**: Sorts the products by total sales in descending order.
- **`.head(10)`**: Selects the top 10 products with the highest sales.
- **`sns.barplot(x=top_products.index, y=top_products.values, palette='rainbow')`**: Creates a bar plot of the top 10 products with a rainbow color palette.

### 7. Customer Segmentation

```python
# Customer Segmentation
customer_summary = df.groupby('CustomerID').agg({'TotalAmount': 'sum', 'InvoiceNo': 'count'}).reset_index()
customer_summary.rename(columns={'TotalAmount': 'TotalSpending', 'InvoiceNo': 'PurchaseFrequency'}, inplace=True)

plt.figure(figsize=(12, 6))
sns.scatterplot(data=customer_summary, x='PurchaseFrequency', y='TotalSpending')
plt.title('Customer Segmentation')
plt.xlabel('Purchase Frequency')
plt.ylabel('Total Spending')
plt.tight_layout()
plt.savefig('images/customer_segmentation.png')  # Save plot as image
plt.close()
```

- **`df.groupby('CustomerID').agg({'TotalAmount': 'sum', 'InvoiceNo': 'count'})`**: Groups the data by `CustomerID` and calculates the total spending and purchase frequency for each customer.
- **`.reset_index()`**: Resets the index to make `CustomerID` a column again.
- **`.rename(columns={'TotalAmount': 'TotalSpending', 'InvoiceNo': 'PurchaseFrequency'})`**: Renames columns for better readability.
- **`sns.scatterplot(data=customer_summary, x='PurchaseFrequency', y='TotalSpending')`**: Creates a scatter plot of purchase frequency vs. total spending.

## Visualizations

### 1. Monthly Sales Over Time

![Monthly Sales Over Time](https://github.com/Sarala23-datascience/Project_4/blob/main/Monthly_Sales_Over_Time.png)

### 2. Top 10 Products by Sales

![Top 10 Products by Sales](https://github.com/Sarala23-datascience/Project_4/blob/main/Top_10_Products_by_Sales.png)

### 3. Customer Segmentation

![Customer Segmentation](https://github.com/Sarala23-datascience/Project_4/blob/main/Customer%20Segmentation.png)


```

