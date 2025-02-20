## Income 💰:

```sql
WITH IncomeGroups AS (
    SELECT 
        CASE 
            WHEN C.YearlyIncome < 50000 THEN 'Low Income (<$50K)'
            WHEN C.YearlyIncome BETWEEN 50000 AND 100000 THEN 'Middle Income ($50K - $100K)'
            ELSE 'High Income (>$100K)'
        END AS IncomeGroup,
        COUNT(C.CustomerKey) AS TotalCustomers,
        SUM(S.SalesAmount) AS TotalSpent,
        AVG(S.SalesAmount) AS AvgSpentPerCustomer
    FROM FactInternetSales S
    JOIN DimCustomer C ON S.CustomerKey = C.CustomerKey
    GROUP BY 
        CASE 
            WHEN C.YearlyIncome < 50000 THEN 'Low Income (<$50K)'
            WHEN C.YearlyIncome BETWEEN 50000 AND 100000 THEN 'Middle Income ($50K - $100K)'
            ELSE 'High Income (>$100K)'
        END
)
SELECT 
    IncomeGroup,
    TotalCustomers,
    TotalSpent,
    AvgSpentPerCustomer,
    LAG(TotalSpent) OVER (ORDER BY TotalSpent) AS PreviousIncomeGroupSpent,
    TotalSpent - LAG(TotalSpent) OVER (ORDER BY TotalSpent) AS DifferenceFromPrevious
FROM IncomeGroups
ORDER BY TotalSpent DESC;
```
| IncomeGroup                  | TotalCustomers                                     | TotalSpent             | AvgSpentPerCustomer      | PreviousIncomeGroupSpent | DifferenceFromPrevious |
|------------------------------|----------------------------------------------------|------------------------|--------------------------|--------------------------|------------------------|
| Middle Income ($50K - $100K) |                                           29,660   |  $        15,051,848   |  $                  507  |  $      10,555,878       |  $       4,495,970     |
| Low Income (<$50K)           |                                           24,655   |  $        10,555,878   |  $                  428  |  $        3,750,951      |  $       6,804,927     |
| High Income (>$100K)         |                                             6,083  |  $          3,750,951  |  $                  617  |  NULL                    |  NULL                  |

We can see that in Average spent, High Income is up. But in total Spent, Middle Income is our batch.
We must decide wich segment to chase. It seems, from this info, that Middle and also Low income are the target ones. 
