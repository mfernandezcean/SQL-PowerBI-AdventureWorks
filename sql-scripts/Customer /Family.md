
```sql
SELECT 
    TotalChildren,
    COUNT(*) AS TotalCustomers,
    SUM(SalesAmount) AS TotalSpent,
    AVG(SalesAmount) AS AvgSpentPerCustomer
FROM FactInternetSales S
JOIN DimCustomer C ON S.CustomerKey = C.CustomerKey
GROUP BY TotalChildren
ORDER BY TotalChildren;
```
