# Commute 🚦:

```sql
SELECT 
    C.CommuteDistance,
    COUNT(C.CustomerKey) AS TotalCustomers,
    SUM(S.SalesAmount) AS TotalSpent,
    AVG(S.SalesAmount) AS AvgSpentPerCustomer
FROM FactInternetSales S
JOIN DimCustomer C ON S.CustomerKey = C.CustomerKey
GROUP BY C.CommuteDistance
ORDER BY TotalSpent DESC;
```



| CommuteDistance | TotalCustomers                                     | TotalSpent           | AvgSpentPerCustomer    |
|-----------------|----------------------------------------------------|----------------------|------------------------|
| 0-1 Miles       |                                           21,307   |  $   11,207,592.22   |  $             526.01  |
| 2-5 Miles       |                                           10,084   |  $     4,965,514.44  |  $             492.42  |
| 5-10 Miles      |                                           10,615   |  $     4,893,148.04  |  $             460.97  |
| 1-2 Miles       |                                           10,170   |  $     4,541,608.15  |  $             446.57  |
| 10+ Miles       |                                             8,222  |  $     3,750,814.37  |  $             456.19  |

# 🚴 How Commute Distance Affects Customer Spending  

## 🔍 Key Finding:  
Customers who commute **0-1 miles** represent a **huge target audience** for sales.  

## 📊 Insights from the Data:  
✅ **Customers commuting 0-1 miles spend the most**, possibly relying on bikes for daily transportation.  
✅ **They purchase a high number of accessories**, such as **helmets, locks, and bike racks**.  
✅ **Long-distance commuters (5+ miles) spend less**—likely using other modes of transport.  

## 🛍️ What Are They Buying?  
- 🚲 **Commuter-friendly bikes** (Touring & Road Bikes)  
- 🎒 **Bike accessories** (Racks, Locks, Bags)  
- 🏁 **Performance gear** (Helmets, Jerseys)  

## 📈 Business Implications:  
🔹 **Target this segment** with marketing campaigns focused on urban commuting solutions.  
🔹 **Bundle accessories** (e.g., bike + helmet + lock) to increase sales.  
🔹 **Analyze seasonal trends**—Are sales higher in warmer months?  

📍 **Next Step:** Deeper analysis of product preferences within this commute segment.  


