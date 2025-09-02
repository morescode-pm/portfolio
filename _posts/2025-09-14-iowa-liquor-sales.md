---
layout: post
tags: [sql, googlebigquery, python, tableau]
title: Iowa Liquor Sales - Trends
published: false
---


BQ has near live data from Iowa Liquor Sales - updated monthly
https://console.cloud.google.com/marketplace/product/iowa-department-of-commerce/iowa-liquor-sales


Iowa Data Center has Census Data
https://www.iowadatacenter.org/index.php/data-by-source/american-community-survey/age-sex-total-population


Assuming people don't leave the county
Assuming they drink every gallon they buy


Usefull Tableau Period Calculation

```sql
-- Calculate previous year and current year
  IF DATEDIFF('month', DATETRUNC('month', [Order Date]), { FIXED : MAX([Order Date])}) <= 11 
    THEN 'CURRENT'
  ELSEIF DATEDIFF('month', DATETRUNC('month', [Order Date]), { FIXED : MAX([Order Date])}) >= 12
    AND DATEDIFF('month', DATETRUNC('month', [Order Date]), { FIXED : MAX([Order Date])}) <= 23 THEN 'PREVIOUS'
  ELSE 'NA'
  END
```
NA is everything more than 2 years old - essentially >= 24 months old and can be excluded.