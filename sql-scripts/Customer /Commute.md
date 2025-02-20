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
