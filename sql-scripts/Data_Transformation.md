
# 📂 Data Transformation  

## 📌 Overview  
Now that we’ve extracted customer, sales, product, and geographic data, we need to **clean, optimize, and reshape** it before using it in Power BI.  

### **🔹 Why Data Transformation Matters?**  
✅ **Improves data quality** by handling NULLs and duplicates  
✅ **Optimizes performance** by pre-aggregating data  
✅ **Prepares structured datasets** for easy reporting  

---

## 🛠️ Key Transformations  

| 📄 File Name                 | 🔍 Description  |
|-----------------------------|------------------------------------------------|
| `clean_customers.sql`        | Handles NULL values, removes duplicates, and standardizes customer data. |
| `format_sales.sql`           | Converts dates, categorizes sales, and creates calculated columns. |
| `aggregate_sales.sql`        | Pre-aggregates total sales per customer for faster Power BI performance. |
| `clean_products.sql`         | Handles missing product categories and standardizes pricing. |
| `optimize_geography.sql`     | Ensures location data consistency and removes missing values. |

---

## **🔹 SQL Scripts for Data Transformation**  

### **✅ Cleaning Customer Data**  
This script **removes duplicates**, **fills missing values**, and **standardizes customer names**:  

```sql
-- 📌 Clean and Standardize Customer Data  
WITH CleanCustomers AS (
    SELECT DISTINCT 
        CustomerKey, 
        COALESCE(FirstName, 'Unknown') AS FirstName,
        COALESCE(LastName, 'Unknown') AS LastName,
        COALESCE(Gender, 'U') AS Gender,  -- 'U' for Unknown
        COALESCE(EmailAddress, 'No Email') AS EmailAddress,
        TotalChildren,
        DateFirstPurchase
    FROM DimCustomer
)
SELECT * FROM CleanCustomers;
```

| CustomerKey | FirstName | LastName | Gender | EmailAddress                   | TotalChildren | DateFirstPurchase |
|-------------|-----------|----------|--------|--------------------------------|---------------|-------------------|
| 11000       | Jon       | Yang     | M      | jon24@adventure-works.com      | 2             | 1/19/2020         |
| 11001       | Eugene    | Huang    | M      | eugene10@adventure-works.com   | 3             | 1/15/2020         |
| 11002       | Ruben     | Torres   | M      | ruben35@adventure-works.com    | 3             | 1/7/2020          |
| 11003       | Christy   | Zhu      | F      | christy12@adventure-works.com  | 0             | 12/29/2019        |
| 11004       | Elizabeth | Johnson  | F      | elizabeth5@adventure-works.com | 5             | 1/23/2020         |

---

-- 📌 Format and Categorize Sales Data  
```sql
-- 📌 Summarize Sales Data by Spending Category  
SELECT 
    SpendingCategory,
    COUNT(DISTINCT CustomerKey) AS TotalCustomers,
    COUNT(SalesOrderNumber) AS TotalOrders,
    SUM(SalesAmount) AS TotalSales,
    AVG(SalesAmount) AS AvgSalesPerOrder
FROM (
    SELECT 
        s.SalesOrderNumber,
        s.CustomerKey,
        s.SalesAmount,
        CASE 
            WHEN s.SalesAmount < 100 THEN 'Low'
            WHEN s.SalesAmount BETWEEN 100 AND 500 THEN 'Medium'
            ELSE 'High' 
        END AS SpendingCategory
    FROM FactInternetSales s
) AS CategorizedSales
GROUP BY SpendingCategory
ORDER BY TotalSales DESC;


```

| SpendingCategory | TotalCustomers      | TotalOrders              | TotalSales              | AvgSalesPerOrder      |
|------------------|---------------------|--------------------------|-------------------------|-----------------------|
| High             |           9,132.0   |                15,205    |  $     28,318,144.7     |  $         1,862.4    |
| Low              |         16,980.0    |                44,616    |  $          961,581.6   |  $              21.6  |
| Medium           |              561.0  |                     577  |  $            78,951.0  |  $            136.8   |
