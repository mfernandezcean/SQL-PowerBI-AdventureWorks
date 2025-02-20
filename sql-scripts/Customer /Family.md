
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


### Let's check the real difference between this 'groups':

```sql
WITH CustomerSpending AS (
    SELECT 
        TotalChildren,
        COUNT(*) AS TotalCustomers,
        SUM(SalesAmount) AS TotalSpent,
        AVG(SalesAmount) AS AvgSpentPerCustomer
    FROM FactInternetSales S
    JOIN DimCustomer C ON S.CustomerKey = C.CustomerKey
    GROUP BY TotalChildren
)
SELECT 
    TotalChildren,
    TotalCustomers,
    TotalSpent,
    AvgSpentPerCustomer,
    LAG(TotalSpent) OVER (ORDER BY TotalChildren) AS PreviousGroupSpent,
    TotalSpent - LAG(TotalSpent) OVER (ORDER BY TotalChildren) AS DifferenceFromPrevious
FROM CustomerSpending
ORDER BY TotalChildren;
```

| TotalChildren | TotalCustomers                                   | TotalSpent             | AvgSpentPerCustomer      | PreviousGroupSpent | DifferenceFromPrevious |
|---------------|--------------------------------------------------|------------------------|--------------------------|--------------------|------------------------|
| 0             |                                    17,048.00     |  $          8,634,027  |  $                  506  |  NULL              |  NULL                  |
| 1             |                                    11,561.00     |  $          6,361,719  |  $                  550  |  $      8,634,027  |  $      (2,272,308)    |
| 2             |                                    12,285.00     |  $          6,327,968  |  $                  515  |  $      6,361,719  |  $           (33,751)  |
| 3             |                                      7,061.00    |  $          3,389,648  |  $                  480  |  $      6,327,968  |  $      (2,938,320)    |
| 4             |                                      7,748.00    |  $          2,937,845  |  $                  379  |  $      3,389,648  |  $         (451,803)   |
| 5             |                                      4,695.00    |  $          1,707,470  |  $                  364  |  $      2,937,845  |  $      (1,230,375)    |
