
# 📂 Data Extraction Scripts  

## 📌 Overview  
This folder contains **SQL scripts** for extracting raw data from the **AdventureWorksDW2022** database. These queries retrieve essential datasets for analysis, ensuring we have **clean and structured data** before transformation.  

## 🔹 Why Data Extraction Matters  
Before performing analysis in **Power BI**, we must:  
✅ **Identify relevant tables** (Fact & Dimension tables)  
✅ **Filter unnecessary data** (Select only required columns & rows)  
✅ **Join related tables** (Using `JOINs` to connect customer, sales, and product data)  
✅ **Optimize queries** (Ensure efficient data retrieval)  

## 📊 Key Scripts in This Folder  
| 📄 File Name | 🔍 Description |
|-------------|--------------|
| `extract_customers.sql` | Retrieves customer details (name, location, demographics). |
| `extract_sales.sql` | Extracts sales transactions from `FactInternetSales`. |
| `extract_products.sql` | Gets product details, categories, and pricing. |
| `extract_geography.sql` | Extracts country, state, and city-level sales data. |

## 🚀 How to Use These Scripts  
1. Open **SQL Server Management Studio (SSMS)**.  
2. Connect to your **AdventureWorksDW2022** database.  
3. Run the scripts in this folder to extract and preview the data.  

> 💡 **Tip:** Save the extracted data as a **view** or **temporary table** for easier access in Power BI.  

---
## 🔹 Extract Customer Sales Summary  

```sql
SELECT c.CustomerKey, c.FirstName, c.LastName, SUM(s.SalesAmount) AS TotalSpent
FROM FactInternetSales s
JOIN DimCustomer c ON s.CustomerKey = c.CustomerKey
GROUP BY c.CustomerKey, c.FirstName, c.LastName
ORDER BY TotalSpent DESC;
```
📊 Query Results
| CustomerKey |First Name  |Last Name | Total Spent |
|--|--|--|--|
12301		|Nichole	|	Nara|	13295.38
12132	|Kaitlyn	|Henderson|	13294.27
12308	|Margaret	|	He	|13269.27
12131	|Randall	|Dominguez	|13265.99
12300	|Adriana	|Gonzalez	|13242.70
12123|	Rosa	|Hu	|13215.65
12124	|Brandi	|Gill	|13195.64
12307	|Brad	|She	|13173.19
12296	|Francisco	|Sara	|13164.64
11433	|Maurice	|Shan	|12909.67

---

```sql
SELECT Top 10 
    s.SalesOrderNumber,
    s.OrderDate,
    c.CustomerKey,
    c.FirstName,
    c.LastName,
    g.SalesTerritoryRegion,
    g.SalesTerritoryCountry,
    s.SalesAmount,
    s.TaxAmt,
    s.Freight
FROM FactInternetSales s
JOIN DimCustomer c ON s.CustomerKey = c.CustomerKey
JOIN DimSalesTerritory g ON s.SalesTerritoryKey = g.SalesTerritoryKey
ORDER BY s.OrderDate DESC;
```

