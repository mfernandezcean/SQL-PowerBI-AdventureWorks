## SQL Script to List Tables in AdventureWorksDW2022

```sql
USE AdventureWorksDW2022;

SELECT TABLE_SCHEMA, TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_TYPE = 'BASE TABLE';
```
## 📊 Tables in AdventureWorksDW2022

| TABLE_SCHEMA | TABLE_NAME |
|-------------|----------------------------------------------|
| dbo         | DatabaseLog                                 |
| dbo         | AdventureWorksDWBuildVersion               |
| dbo         | DimAccount                                  |
| dbo         | DimCurrency                                 |
| dbo         | DimCustomer                                 |
| dbo         | DimDate                                     |
| dbo         | DimDepartmentGroup                         |
| dbo         | DimEmployee                                |
| dbo         | DimGeography                               |
| dbo         | DimOrganization                            |
| dbo         | DimProduct                                 |
| dbo         | DimProductCategory                         |
| dbo         | DimProductSubcategory                      |
| dbo         | DimPromotion                               |
| dbo         | DimReseller                                |
| dbo         | DimSalesReason                             |
| dbo         | DimSalesTerritory                          |
| dbo         | DimScenario                                |
| dbo         | FactAdditionalInternationalProductDescription |
| dbo         | FactCallCenter                             |
| dbo         | FactCurrencyRate                           |
| dbo         | Employees                                  |
| dbo         | FactFinance                                |
| dbo         | FactInternetSales                          |
| dbo         | FactInternetSalesReason                   |
| dbo         | FactProductInventory                      |
| dbo         | FactResellerSales                         |
| dbo         | FactSalesQuota                            |
| dbo         | FactSurveyResponse                        |
| dbo         | NewFactCurrencyRate                       |
| dbo         | ProspectiveBuyer                          |
