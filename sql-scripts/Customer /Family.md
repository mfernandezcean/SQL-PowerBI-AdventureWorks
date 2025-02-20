
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
| TotalChildren | TotalCustomers                                     | TotalSpent           | AvgSpentPerCustomer    |
|---------------|----------------------------------------------------|----------------------|------------------------|
| 0             |                                           17,048   |  $     8,634,026.94  |  $             506.45  |
| 1             |                                           11,561   |  $     6,361,718.95  |  $             550.27  |
| 2             |                                           12,285   |  $     6,327,967.71  |  $             515.10  |
| 3             |                                             7,061  |  $     3,389,648.09  |  $             480.05  |
| 4             |                                             7,748  |  $     2,937,845.20  |  $             379.17  |
| 5             |                                             4,695  |  $     1,707,470.34  |  $             363.68  |
