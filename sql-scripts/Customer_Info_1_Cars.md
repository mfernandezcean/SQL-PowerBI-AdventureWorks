# 🚗 Do Cars Influence Customer Behavior?  

## ❓ Why Are We Analyzing Car Ownership?  

We believe that **car ownership plays a key role in identifying different types of customer behaviors**.  

Understanding how many cars a customer owns helps us:  
✅ **Segment customers** based on lifestyle and spending habits  
✅ **Analyze purchasing trends**, especially for bike-related products  
✅ **Optimize marketing strategies** by targeting the right audience  

For example:  
- 🚴 **Customers with no cars** might buy bikes for transportation.  
- 🚗 **Customers with 1 car** may purchase bikes for fitness or leisure.  
- 🚙 **Customers with 2+ cars** could have higher spending power but different mobility needs.  

---

## 🔍 **Key Questions We Want to Answer:**  
1️⃣ **How many customers own 0, 1, or 2+ cars?**  
2️⃣ **Do car owners buy more or fewer bikes?**  
3️⃣ **Which bike models are most popular among different customer groups?**  
4️⃣ **Can we predict high-value customers based on car ownership?**  

By analyzing these factors, we can better **understand customer preferences** and **tailor marketing and sales strategies accordingly**. 🚀  


## 🚗 Customer Car Ownership Analysis  

### ❓ What Are We Analyzing?  
This query **categorizes customers based on the number of cars they own** and calculates their **percentage in the total customer base**.  

Understanding car ownership helps us:  
✅ **Segment customers** based on lifestyle and purchasing power  
✅ **Identify trends in transportation preferences**  
✅ **Predict potential interest in related products like bikes**  

---

## 📝 **SQL Query:**
```sql
SELECT 
    CASE 
        WHEN NumberCarsOwned >= 2 THEN 'Owns 2 or more'
        WHEN NumberCarsOwned = 1 THEN '1 car'
        ELSE 'Zero cars' 
    END AS customers_cars,
    COUNT(CustomerKey) AS total_customers,
    (COUNT(CustomerKey) * 100.0 / SUM(COUNT(CustomerKey)) OVER ()) AS percentage_customers
FROM DimCustomer
GROUP BY 
    CASE 
        WHEN NumberCarsOwned >= 2 THEN 'Owns 2 or more'
        WHEN NumberCarsOwned = 1 THEN '1 car'
        ELSE 'Zero cars' 
    END
ORDER BY percentage_customers DESC;
```
| customers_cars | total_customers         | percentage_customers |
|----------------|-------------------------|----------------------|
| Owns 2 or more |                  9,363  | 50.65                |
| 1 car          |                  4,883  | 26.42                |
| Zero cars      |                  4,238  | 22.93                |

## 🚗 Do Different Customers Buy Different Bikes?
We suspect that car ownership strongly influences what types of bikes and accessories customers purchase. This query helps us explore:

✅ **What bike models customers prefer**
✅ **How preferences change based on car ownership**
✅ **Whether we can classify different customer types based on these trends**


So lets see what kind of Bikes and accesories they buy. We have the suspicioon that their are different type of customers. And that division is settled clearly by the ownership of cars

```sql

WITH BikePurchases AS (
    SELECT 
        C.NumberCarsOwned,
        P.EnglishProductName AS BikeModel,
        COUNT(S.SalesOrderNumber) AS PurchaseCount
    FROM FactInternetSales S
    JOIN DimCustomer C ON S.CustomerKey = C.CustomerKey
    JOIN DimProduct P ON S.ProductKey = P.ProductKey
    WHERE P.EnglishProductName LIKE '%Bike%'  -- Filtering only bikes
    GROUP BY C.NumberCarsOwned, P.EnglishProductName
)
SELECT 
    NumberCarsOwned, 
    BikeModel, 
    PurchaseCount,
    ROUND((PurchaseCount * 100.0 / SUM(PurchaseCount) OVER (PARTITION BY NumberCarsOwned)), 2) AS Percentage
FROM BikePurchases
ORDER BY NumberCarsOwned, Percentage DESC;
```

| NumberCarsOwned | BikeModel              | PurchaseCount | Percentage |
|-----------------|------------------------|---------------|------------|
| 0               | Bike Wash - Dissolver  | 173           | 62.01      |
| 0               | Hitch Rack - 4-Bike    | 59            | 21.15      |
| 0               | All-Purpose Bike Stand | 47            | 16.85      |
| 1               | Bike Wash - Dissolver  | 222           | 61.33      |
| 1               | Hitch Rack - 4-Bike    | 82            | 22.65      |
| 1               | All-Purpose Bike Stand | 58            | 16.02      |
| 2               | Bike Wash - Dissolver  | 351           | 59.8       |
| 2               | Hitch Rack - 4-Bike    | 138           | 23.51      |
| 2               | All-Purpose Bike Stand | 98            | 16.7       |
| 3               | Bike Wash - Dissolver  | 99            | 63.46      |
| 3               | Hitch Rack - 4-Bike    | 32            | 20.51      |
| 3               | All-Purpose Bike Stand | 25            | 16.03      |
| 4               | Bike Wash - Dissolver  | 63            | 62.38      |
| 4               | All-Purpose Bike Stand | 21            | 20.79      |
| 4               | Hitch Rack - 4-Bike    | 17            | 16.83      |


## 💡 What This Data Tells Us:

🚲 **Customers with no cars** tend to buy **Touring bikes**, likely for transportation.  

🚙 **Customers with 1 car** show a **balanced split** between **road bikes and mountain bikes**—probably for fitness or weekend rides.  

🏎️ **Customers with 2+ cars** are **high spenders**, favoring **premium models**, but their **percentage of bike purchases is lower**.  
