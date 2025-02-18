
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
##  🔹 Extract the Top 10 Sales Transactions
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
| SalesOrderNumber | OrderDate | CustomerKey | FirstName | LastName  | SalesTerritoryRegion | SalesTerritoryCountry | SalesAmount | TaxAmt | Freight |
|------------------|-----------|-------------|-----------|-----------|----------------------|-----------------------|-------------|--------|---------|
| SO75084          | 00:00.0   | 11078       | Gina      | Martin    | Canada               | Canada                | 120         | 9.6    | 3       |
| SO75085          | 00:00.0   | 11927       | Nicole    | Murphy    | Northwest            | United States         | 8.99        | 0.7192 | 0.2248  |
| SO75085          | 00:00.0   | 11927       | Nicole    | Murphy    | Northwest            | United States         | 7.95        | 0.636  | 0.1988  |
| SO75086          | 00:00.0   | 28789       | Elijah    | Zhang     | Southwest            | United States         | 7.95        | 0.636  | 0.1988  |
| SO75087          | 00:00.0   | 11794       | Lauren    | Ross      | Southwest            | United States         | 34.99       | 2.7992 | 0.8748  |
| SO75088          | 00:00.0   | 14680       | Marvin    | Munoz     | Australia            | Australia             | 3.99        | 0.3192 | 0.0998  |
| SO75088          | 00:00.0   | 14680       | Marvin    | Munoz     | Australia            | Australia             | 24.99       | 1.9992 | 0.6248  |
| SO75088          | 00:00.0   | 14680       | Marvin    | Munoz     | Australia            | Australia             | 34.99       | 2.7992 | 0.8748  |
| SO75088          | 00:00.0   | 14680       | Marvin    | Munoz     | Australia            | Australia             | 49.99       | 3.9992 | 1.2498  |
| SO75089          | 00:00.0   | 19585       | Kristi    | Fernandez | Australia            | Australia             | 21.49       | 1.7192 | 0.5373  |

---

## 📌 Handling NULL Values in Product Data  

We noticed that our **product data contains many NULL values**, especially in **categories, subcategories, and pricing fields**.  

### **🔹 Why Does This Happen?**  
NULL values appear when:  
- Some products **don’t have assigned categories or subcategories**.  
- Data is **incomplete or missing** in the source tables.  
- We use **LEFT JOIN**, which includes all products even if they have no matching categories.  

---

### **🔹 Different Approaches to Handle NULLs**  
When dealing with missing data, we have several options:  

1️⃣ **Remove NULL values entirely** (Use `INNER JOIN`)  
2️⃣ **Fill missing values with placeholders** (Use `COALESCE()`)  
3️⃣ **Impute missing values using averages, medians, or defaults**  

👉 Learn more about handling NULLs: [Prepping Data](https://www.preppindata.com/howto/how-to-deal-with-nulls)  

---

### **✅ Our Solution: Using COALESCE()**  
We chose **COALESCE()** because:  
✔️ **Fast** → Simple and efficient SQL function  
✔️ **Clear** → Makes missing data easy to understand  
✔️ **Keeps all products** → Even those without categories  

```sql
SELECT 
    p.ProductKey,
    p.EnglishProductName AS ProductName,
    COALESCE(p.Color, 'No Color') AS Color,
    COALESCE(p.StandardCost, 0) AS StandardCost,
    COALESCE(p.ListPrice, 0) AS ListPrice,
    COALESCE(sc.EnglishProductSubcategoryName, 'No Subcategory') AS Subcategory,
    COALESCE(c.EnglishProductCategoryName, 'No Category') AS Category
FROM DimProduct p
LEFT JOIN DimProductSubcategory sc ON p.ProductSubcategoryKey = sc.ProductSubcategoryKey
LEFT JOIN DimProductCategory c ON sc.ProductCategoryKey = c.ProductCategoryKey
ORDER BY c.EnglishProductCategoryName, sc.EnglishProductSubcategoryName, p.EnglishProductName;
```
| ProductKey | ProductName     | Color | StandardCost | ListPrice | Subcategory    | Category    |
|------------|-----------------|-------|--------------|-----------|----------------|-------------|
| 1          | Adjustable Race | NA    | 0            | 0         | No Subcategory | No Category |
| 3          | BB Ball Bearing | NA    | 0            | 0         | No Subcategory | No Category |
| 2          | Bearing Ball    | NA    | 0            | 0         | No Subcategory | No Category |
| 5          | Blade           | NA    | 0            | 0         | No Subcategory | No Category |
| 13         | Chain Stays     | NA    | 0            | 0         | No Subcategory | No Category |

---

```sql
SELECT 
    g.GeographyKey,
    g.CountryRegionCode AS Country,
    g.StateProvinceName AS State,
    g.City,
    SUM(s.SalesAmount) AS TotalSales
FROM FactInternetSales s
JOIN DimCustomer c ON s.CustomerKey = c.CustomerKey
JOIN DimGeography g ON c.GeographyKey = g.GeographyKey
GROUP BY g.GeographyKey, g.CountryRegionCode, g.StateProvinceName, g.City
ORDER BY TotalSales DESC;
```
| GeographyKey | Country | State           | City        | TotalSales       |
|--------------|---------|-----------------|-------------|------------------|
| 19           | AU      | New South Wales | Wollongong  |  $   338,913.47  |
| 40           | AU      | Victoria        | Warrnambool |  $   327,036.37  |
| 32           | AU      | Victoria        | Bendigo     |  $   314,568.72  |
| 4            | AU      | New South Wales | Goulburn    |  $   310,875.90  |
| 298          | US      | California      | Bellflower  |  $   302,278.81  |
| 20           | AU      | Queensland      | Brisbane    |  $   295,353.58  |
| 27           | AU      | Queensland      | Townsville  |  $   285,486.91  |
| 34           | AU      | Victoria        | Geelong     |  $   283,802.18  |
| 21           | AU      | Queensland      | Caloundra   |  $   281,986.34  |
| 18           | AU      | New South Wales | Sydney      |  $   280,983.31  |
