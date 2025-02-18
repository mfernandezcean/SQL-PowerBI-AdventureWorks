
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
