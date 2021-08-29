# World-Data-of-Health
Exploring world Health Data

# World Health_Nutrition Dataset

# Average Birth Rate in Various Countries arranged in descending with maximum and minimum birth rate recorded
```sql

SELECT 
country_name,
avg(value) as Birth_rate
FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where indicator_name = 'Birth rate, crude (per 1,000 people)'
group by country_name
order by Birth_rate desc

```
![image](https://user-images.githubusercontent.com/89662666/131261018-6576dcfc-e2b3-41eb-9edb-caa18abb5bdb.png)




# Countries with Highest Average Mortality rate amongst infants per 1000 live Births. Also minimum and maximum mortality rate recorded in these countries 
```sql

SELECT 
country_name,
avg(value) as Avg_Mortality_rate,
MIN(value) AS Min_Mortality_rate,
MAX(value) AS Max_Mortality_rate,
FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where indicator_name ='Mortality rate, infant (per 1,000 live births)'
group by country_name
order by Avg_Mortality_rate desc

```
![image](https://user-images.githubusercontent.com/89662666/131260917-faba6437-b551-4b30-827a-3990bfd4a89a.png)


### Countries where minimum mortality rate is too high
```sql

SELECT 
country_name,year,
MIN(value) AS Min_Mortality_rate
FROM `bigquery-public-data.world_bank_health_population.health_nutrition_population` 
where indicator_name ='Mortality rate, infant (per 1,000 live births)'
group by country_name,year
order by Min_Mortality_rate desc
```
![image](https://user-images.githubusercontent.com/89662666/131260892-49dffc72-d8b5-4aae-8bad-53332903776e.png)

# Health expenditure of the country where mortality rate is higher than global average in 2017

```sql

with Avg_Mortality as (
SELECT
avg(value) as Avg_Mortality_rate
FROM
  `bigquery-public-data.world_bank_health_population.health_nutrition_population`
where indicator_name = 'Mortality rate, infant (per 1,000 live births)'
and year = 2017
),

list_of_countries as 
(select 
distinct (country_name),value 
FROM
  `bigquery-public-data.world_bank_health_population.health_nutrition_population`
join Avg_Mortality  a
on 1 = 1 
where indicator_name = 'Mortality rate, infant (per 1,000 live births)'
and year = 2017
and value > a.Avg_Mortality_rate)

select loc.country_name, world.value as health_expenditure_per_capita
FROM list_of_countries loc 
join `bigquery-public-data.world_bank_health_population.health_nutrition_population` world
on world.country_name = loc.country_name 
where indicator_name = 'Current health expenditure per capita (current US$)'
and year = 2017
order by health_expenditure_per_capita desc

```
![image](https://user-images.githubusercontent.com/89662666/131260809-63ed1fd8-c34d-4d89-906f-180a5760c9f7.png)

# Countries which has the highest level of HIV incidence and in which year
```sql

SELECT
country_name,year,
 
max(value) as  Incidence_of_HIV
from`bigquery-public-data.world_bank_health_population.health_nutrition_population`
where indicator_name = 'Incidence of HIV, all (per 1,000 uninfected population)'
and value   > 0 
group by  country_name, year
order by Incidence_of_HIV desc
limit 1

```
![image](https://user-images.githubusercontent.com/89662666/131260766-7b800499-d6c6-4cef-86a7-3612a7fe8a4d.png)



