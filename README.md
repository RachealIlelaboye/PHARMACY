# ABC PHARMACEUTICAL COMPANY
An end-to-end data analysis project using Excel, Python, SQL, and Power BI to analyze sales data, identify trends, and generate interactive dashboards

# PROJECT OVERVIEW

## Aim
This project aims to overcome these challenges by conducting a 
comprehensive data and geospatial analysis, revealing intricate patterns and correlations. By 
leveraging advanced analytical techniques, the company can uncover hidden insights, refine 
marketing strategies, and improve overall operational efficiency.

## Objectives
The objectives of this analysisis to:
- **Identify Sales Trends:** Uncover trends in sales data over time, including peak periods and 
sales dips.
- **Understand Customer Behavior:** Examine customer demographics and purchasing patterns 
to identify key customer segments and behaviors.
- **Evaluate Product Performance:** Assess the performance of different products, identifying 
best-sellers and underperforming items.
- **Analyze Geospatial Impact:** Understand the impact of geography on sales to identify high 
and low-performing regions.
- **Channel Performance:** Compare sales performance across different customer channels 
(e.g., Hospital vs. Pharmacy).
- **Optimize Sales Strategies:** Provide recommendations for improving sales strategies based 
on data insights.
- **Enhance Product Offerings:** Suggest optimizations for product offerings based on 
performance and customer preferences.

# DATA SOURCE
The dataset for this analysis is an excel file gotten from 3Signet [Official Website](https://www.3signet.com/)

#TOOLS USED
- MS Excel [Download here](https://www.microsoft.com/en-us/microsoft-365/excel)

  Initial data cleaning and exploration

- DB SQLITE [Download here](https://sqlitebrowser.org/dl/)

   Querying and aggregating data from a relational database

- Python [Download here](https://www.python.org/downloads/)

   Explorative Data Analysis

- PowerBI [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

   Building interactive dashboards and visualizations

- MS Powerpoint [Download here](https://www.microsoft.com/en/microsoft-365/powerpoint)

   Presentation slide preparation

- Github [Signup here](https://github.com/join)

  Portfolio building

- Zoom [Download here](https://zoom.us/download?os=win)

   Live demonstration

#   WEEK 7
- SQL database set up using SQLite DB Browser for the pharmaceutical data, ensuring 
data integrity and security

- Creation of facts and dimension tables
```SQL
-------------Creating fact table
CREATE TABLE Sales(
    SalesId INTEGER PRIMARY KEY NOT NULL,
	Sales 	INTEGER  NOT NULL,
    Month 	TEXT  NOT NULL,
	Year date  NOT NULL,
	ProductId INTEGER  NOT NULL,
	CustomerId INTEGER  NOT NULL,
	Name_of_sales_Rep TEXT  NOT NULL,
	Manager TEXT  NOT NULL,
	Sales_Team TEXT NOT NULL,
	
	FOREIGN KEY (ProductId) REFERENCES Products(ProductId),
	FOREIGN KEY (CustomerId) REFERENCES Location (CustomerId));
	
	-------Dimension Tables
	CREATE TABLE Products(
	ProductName TEXT NOT NULL,
	ProductClass TEXT NOT NULL,
	Quantity_Price INTEGER NOT NULL,
	ProductId INTEGER PRIMARY KEY NOT NULL);
	
  CREATE TABLE Location(
	Distributor TEXT NOT NULL,
	CustomerId INTEGER PRIMARY KEY NOT NULL,
	CustomerName TEXT NOT NULL,
	City TEXT NOT NULL,
	CHANNEL TEXT NOT NULL,
	COUNTRY TEXT NOT NULL,
	Sub_channel TEXT NOT NULL,
	Latitude INTEGER NOT NULL,
	Longitude  INTEGER NOT NULL);
```
- ERD Creation using SQLITE
![Pharm ERD Racheal Ilelaboye](https://github.com/user-attachments/assets/c8531558-70d2-47ae-a69e-50d5977681fb)


- Initial data cleaning
```SQL
---- Count missing values in each column
SELECT 
    COUNT(*) - COUNT(Sales) AS MissingValues, 
    Sales
FROM 
   Pharm_Data ;
```

# WEEK 8
- Sales performance analysis
```SQL
---------Total sales by Products
SELECT 
    ProductName,  SUM(Sales) AS TotalSales
FROM 
    Pharm_Data
GROUP BY  
    ProductName
ORDER BY 
    TotalSales DESC;
```
![1A](https://github.com/user-attachments/assets/3b67a47d-813f-4876-83ff-0b76380d120b)

```SQL
----------MONTHLY SALES TREND
SELECT 
    Month,
    SUM(Sales) AS TotalMonthlySales
FROM 
    Pharm_Data
GROUP BY 
    Month
ORDER BY 
     Month;
```
![1B](https://github.com/user-attachments/assets/18c446da-cb61-4bc6-8e62-8132ec553681)

- Sales by location
```SQL
-----CITIES WITH HIGH SALES VOLUME
SELECT 
    City, 
    SUM(Sales) AS TotalSalesVolume
FROM 
    Pharm_Data
GROUP BY 
    City
ORDER BY 
    TotalSalesVolume DESC
LIMIT 10;  
```
![2A](https://github.com/user-attachments/assets/011c328d-52f9-4d78-b621-1af23dbde71a)

```SQL
--------sales by Country
SELECT 
    Country, 
    SUM(Sales) AS TotalSalesVolume
FROM 
    Pharm_Data
GROUP BY 
    Country
ORDER BY 
    TotalSalesVolume DESC;  
```
![2B](https://github.com/user-attachments/assets/570441db-4664-48c4-9474-338c0e778aa6)
- Customer Segmentation
```SQL
------Total sales by customer type
SELECT 
    Channel, 
    SUM(Sales) AS TotalSalesVolume
FROM 
    Pharm_Data
GROUP BY 
    Channel
ORDER BY 
    TotalSalesVolume DESC;
```
![3A](https://github.com/user-attachments/assets/45d67760-e31d-4b55-aada-90ea5a8824f7)
```SQL
------Total sales by sector
SELECT 
    Sub_channel AS Sector, 
    SUM(Sales) AS TotalSales
FROM 
    Pharm_Data
GROUP BY 
    Sub_channel
ORDER BY 
    TotalSales DESC;
```
![3B](https://github.com/user-attachments/assets/edd9639d-7f54-4afb-94e0-53453e388f5a)
- Product analysis
``` SQL
---------top selling products within each drug class
WITH RankedProducts AS (
    SELECT 
        ProductClass,
        ProductName,
        SUM(Sales) AS TotalSales,
        RANK() OVER (PARTITION BY ProductClass ORDER BY SUM(Sales) DESC) AS SalesRank
    FROM 
        Pharm_Data
    GROUP BY 
        ProductClass, 
        ProductName)
SELECT 
    ProductClass,
    ProductName,
    TotalSales
FROM 
    RankedProducts
WHERE 
    SalesRank = 1
ORDER BY 
    ProductClass, TotalSales DESC;
```
![4A](https://github.com/user-attachments/assets/f47e060e-2b82-4657-a1a0-3c04e738b571)
```SQL
----average price for each drug class
SELECT 
    ProductClass,  avg(Price) AS Averageprice
FROM 
    Pharm_Data
GROUP BY  
    ProductClass
ORDER BY 
    Averageprice DESC;
```
![4B](https://github.com/user-attachments/assets/a6dcc412-286e-4046-ab52-97161da172df)
- Sales representative performance
```sql
------top performing sales rep
SELECT 
    NameofSalesRep,  sum(Sales) AS Totalsales
FROM 
    Pharm_Data
GROUP BY  
   NameofSalesRep
ORDER BY 
    Totalsales DESC
	LIMIT 10;
```
[5A](https://github.com/user-attachments/assets/5aa3a322-fbe0-4308-bc9c-3707a7ce0137)
```sql
------sales across teams
SELECT 
    SalesTeam,  sum(Sales) AS Totalsales
FROM 
    Pharm_Data
GROUP BY  
   SalesTeam
ORDER BY 
    Totalsales DESC;
```
![5B](https://github.com/user-attachments/assets/31529632-c5d9-4502-9e0f-9ad563c7f97b)
- Time series analysis
```sql
-----year over year growth in sales
WITH YearlySales AS (
    SELECT 
        Year,
        SUM(Sales) AS TotalSales
    FROM 
        Pharm_Data
    GROUP BY 
        Year
)
SELECT 
    Year,
    TotalSales,
    LAG(TotalSales) OVER (ORDER BY Year) AS PreviousYearSales,
    CASE 
        WHEN LAG(TotalSales) OVER (ORDER BY Year) IS NOT NULL THEN 
            ((TotalSales - LAG(TotalSales) OVER (ORDER BY Year)) / LAG(TotalSales) OVER (ORDER BY Year)) * 100
        ELSE 
            NULL
    END AS YoYGrowth
FROM 
    YearlySales
ORDER BY 
    Year;
```
![6A](https://github.com/user-attachments/assets/601ce23b-5d7b-487d-8ec5-9b4ebf712848)
```sql
----------seasonal sales patterns
SELECT 
    Month,
    SUM(Sales) OVER (PARTITION BY Month) AS MonthlySales
FROM 
    Pharm_Data
GROUP BY 
    YEAR, MONTH
ORDER BY 
    MonthlySales DESC;
```
![6B](https://github.com/user-attachments/assets/59be3ee9-0988-40b5-9a43-d1e40165c5cf)

# WEEK 9
- Data validation
```python
# Connect to database
conn = r"C:\Users\user\Desktop\3Signet\Week 7\Pharm.db"
connection = sqlite3.connect(conn)
# Read sqlite query results into a pandas DataFrame
connection = sqlite3.connect(r"C:\Users\user\Desktop\3Signet\Week 7\Pharm.db" )
df = pd.read_sql_query("SELECT * from Pharm_Data", connection)
```

```python
#To check for null values
df.isna().sum()

# Check for duplicates in the entire dataset
duplicates = df.duplicated()
print(f"Number of duplicate rows: {duplicates.sum()}")

# To confirm datatype consistency
print(df.dtypes)

## Using Z-scxore to identify outliers
from scipy import stats
df['z_score'] = np.abs(stats.zscore(df['Sales']))
outliers_z = df[df['z_score'] > 3]
print(outliers_z)

# Replacing outliers
df['Sales'] = np.where(df['Sales'] > upper_bound, upper_bound,
np.where(df['Sales'] < lower_bound, lower_bound,␣
↪df['Sales']))
```
- Explorative data analysis
```python
 # Descriptive statistics
df.describe().round(1)

# Filter negative sales values
negative_sales = df[df['Sales'] < 0]
print(negative_sales)

# DATA VALIDATION
# Create a new column for returns (negative values)
df['Returns'] = df['Sales'].apply(lambda x: x if x < 0 else 0)
# Create a new column for positive sales

# Summary of data validation checks
validation_report = {
'Missing Values': df.isnull().sum(),
'Duplicates': df.duplicated().sum(),
'Invalid Data Types': df.dtypes,
'Outliers': df[df['z_score'].abs() > 3].shape[0]
}
print(validation_report)
```

