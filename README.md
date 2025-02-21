## 📌 About AdventureWorksDW2022  

🚀 **The Ultimate Playground for Data Analysts & BI Professionals**  

The **AdventureWorksDW2022** database is a **realistic, enterprise-level** data warehouse provided by Microsoft. It serves as a **gold-standard dataset** for anyone looking to master **SQL, Power BI, and data-driven decision-making**.  

Whether you're a **data analyst, business intelligence professional, or a Power BI enthusiast**, this dataset gives you the **perfect foundation to practice data extraction, transformation, and visualization.**  

### 🔹 Why is AdventureWorksDW2022 Special?  

✅ **Enterprise-Level Data Warehouse** → Simulates a real-world business scenario  
✅ **Star Schema Design** → Perfect for reporting, aggregations, and fast queries  
✅ **Comprehensive Sales Data** → Track customer orders, revenue, and trends  
✅ **Time Intelligence Ready** → Pre-built **date dimensions** for time-series analysis  
✅ **Multi-Dimensional Insights** → Explore data across **customers, products, and geography**  

**In short:** This dataset provides everything needed to **extract insights, build reports, and create compelling dashboards** that mimic real-world business operations.  

---
## 📊💰 Business Insights  
### 🚴 How Commute Distance Affects Customer Spending  
💡 **Key Finding:**  
Customers who commute **0-1 miles** spend **30% more** on bike-related purchases.  

📊 **Business Impact:**  
- **Urban commuters represent a high-value customer segment** for bike retailers.  
- **Short-distance customers buy more accessories** (helmets, locks, and bags).  
- **Long-distance commuters (5+ miles) spend less on bikes**, likely relying on cars/public transport.  

🎯 **Recommendations:**  
✅ Create **"Urban Commuter Bundles"** (bike + lock + helmet).  
✅ Run **geo-targeted marketing campaigns** in cities.  
✅ Expand accessory offerings (smart locks, bike racks).  

🔗 **Full SQL analysis here:** [\[Commute.md\]](https://github.com/mfernandezcean/SQL-PowerBI-AdventureWorks/blob/main/sql-scripts/Customer%20/Commute.md)

💡 **Future Analysis:**  
- Analyze how **income level & commute distance** together influence purchases.  
- Explore **seasonal trends** in commuter spending.  

---

### 📊 What's Inside AdventureWorksDW2022?  

The database is structured into **fact and dimension tables**, making it ideal for **OLAP (Online Analytical Processing) and reporting applications.**  

#### 🔹 **Fact Tables (The Numbers that Matter!)**  
These store transactional data like sales, financials, and production numbers:  
- `FactInternetSales` → Online sales transactions 💰  
- `FactResellerSales` → B2B reseller sales 🏢  
- `FactFinance` → Financial transactions & budget analysis 📊  

#### 🔹 **Dimension Tables (The Story Behind the Numbers!)**  
These provide context to the numbers, allowing **deeper insights**:  
- `DimCustomer` → Customer demographics & purchasing behavior 🛒  
- `DimProduct` → Detailed product data, categories & pricing 🎯  
- `DimDate` → Pre-built date table for easy time-series analysis 📆  
- `DimGeography` → Location-based insights (Country, State, City) 🌍  

**💡 Why is this important?**  
👉 With these tables, we can analyze **sales trends**, **customer behavior**, and **geographic revenue distribution**—all key elements of a **BI-driven strategy.**  

---

### 🚀 How We’ll Use AdventureWorksDW2022 in This Project  
# Our focus? ****Customers.****

We’ll explore **who buys the most, what they buy, and where they are located** using **SQL and Power BI**.  

🔹 **Extract** customer sales data with SQL  
🔹 **Transform** raw data into actionable insights  
🔹 **Visualize** trends and key metrics in Power BI  

By the end of this project, we’ll have a **professional-grade dashboard** that provides **clear, data-driven business insights**.  

📌 **Follow along and level up your BI skills! 🚀**  

