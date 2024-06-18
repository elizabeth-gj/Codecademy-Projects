## 📺 Codeflix: Calculating Churn Rates

## Data Table
Four months into launching Codeflix, we want to investigate subscription churn rates to understand how the company is doing - particularly if there is any difference in between subscriber's segments. Codeflix requires a minimum subscription length of 31 days, so a user can never start and end their subscription in the same month. 

We get subscription data in a `subscriptions` table which is made of 4 columns:
- `id` : subscription_id
- `subscription_start`
- `subscription_end`
- `segment` : which identifies which segment the subscriber belongs to.

***Data Set:*** The `subscriptions` file is attached :
[Codecademy-Projects _ Codeflix Churn Rates - Feuille 1.csv](https://github.com/user-attachments/files/15890222/Codecademy-Projects._.Codeflix.Churn.Rates.-.Feuille.1.csv)

***Tools:*** The full SQL script is [here](). More information on how I built the query step by step below.
***

## Explorating the Data:
Let's explore the data first:

```sql
SELECT *
FROM subscriptions
LIMIT 10;
```
```sql
SELECT COUNT(*)  AS count_subscriptions
FROM subscriptions;
```
```sql
SELECT segment
     , COUNT(*) AS count_subscriptions
FROM subscriptions
GROUP BY 1;
```

**Outcome:**
The table is quite simple indeed with only 4 columns: first is integer, col 2 and 3 are dates and last one is integer. There are in total 2,000 subscriptions - perfectly divided into 2 segments called "30" and "87".
![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/a329c1e1-3fa3-4fbe-bf79-e88668929ec7)
![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/2cef6a6f-0016-4fd8-b971-1ffb41992da8)
![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/e3d01e63-d3c0-4bce-8467-f91a55c7bd93)


## Calculating  Churn Rate per Segment:

Ok, let's start thinking about creating Churn Rate calculations.

What I need first is to understand if a customer is active or canceled - and for this I'll create a `months` table for the first 3 months of the year, the range I want to analyze - to work as a reference I can then compare subscriptin_start and subscription_end dates with:

```sql
WITH months AS (
SELECT
'2017-01-01' AS first_day,
'2017-01-31' AS last_day
UNION
SELECT
'2017-02-01' AS first_day,
'2017-02-28' AS last_day
UNION
SELECT
'2017-03-01' AS first_day,
'2017-03-31' AS last_day
)
....
```

I then join this table to the `subscriptions` table with a CROSS JOIN 

```sql
...
,subscriptions_and_months AS (
SELECT *
FROM subscriptions 
CROSS JOIN months
)
```

I know can get into grouping data per users per segments and establishing their status with the help of a CASE WHEN grouping:
```sql
...
,status AS (
SELECT id
     , first_day AS month
     , CASE WHEN (segment = 87) AND (subscription_start < first_day) AND (subscription_end > first_day OR subscription_end IS NULL) THEN 1 ELSE 0 END AS is_active_87,
       CASE WHEN (segment = 30) AND (subscription_start < first_day) AND (subscription_end > first_day OR subscription_end IS NULL) THEN 1 ELSE 0 END AS is_active_30,
       CASE WHEN (segment = 87) AND (subscription_end BETWEEN first_day AND last_day) THEN 1 ELSE 0 END AS is_canceled_87,
       CASE WHEN (segment = 30) AND (subscription_end BETWEEN first_day AND last_day) THEN 1 ELSE 0 END AS is_canceled_30
FROM cross_join
)
```

Now, I want to move up in granularity: from users to month, thanks to another CTE:
```sql
...
,status_aggregate AS (
SELECT month
     , SUM(is_active_87) AS sum_active_87
     , SUM (is_active_30) AS sum_active_30
     , SUM(is_canceled_87) AS sum_canceled_87
     , SUM (is_canceled_30) AS sum_canceled_30
FROM status
GROUP BY month
)
```

And finally, I can get to calculate churn rate per segments and months:
```sql
...
SELECT month
     , ROUND(1.0*sum_canceled_30/sum_active_30,2) AS churn_rate_30
     , ROUND(1.0*sum_canceled_87/sum_active_87,2) AS churn_rate_87
FROM status_aggregate;
```

**Outcome:**

The segment with the lowest churn rate is ***segment 30***, with on average a churn rate 4 times inferior to segment 87 churn rate.
This information would be very valuable to the marketing team to understand that segment 87 might not be as of a good investment as segment 30.

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/992628cc-4a37-4732-8844-f27a32de2b4c)
