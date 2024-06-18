## 👓 Warby Parker: Conversion Rates, Marketing Funnels, A/B tests

## Data Table
[Warby Parker](https://www.warbyparker.com/) offers the world's most affordable eyewear. The brand was built from a strong DTC presence and that's what we're going to look at today.

We will analyze different marketing funnels and calculate conversion rates.

Quiz Funnel: 
- `survey` table

Home-Try-On Funnel: 
- `quiz`, `home_try_on` and `purchase` tables


***

## Quiz Funnel:
This table records data when users complete [this following quiz](https://www.warbyparker.com/quiz/hto) on Warby Parker's website.
Let's explore the data first:

```sql
SELECT *
FROM survey
LIMIT 10;
```
This table is quite simple with question | user_id | response
![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/a30aaa72-ef53-4abb-aa61-a89515d30811)

What are the completion rates as we move from question 1 to 2 to 3 ... to 5?

```sql
SELECT question
     , COUNT(DISTINCT user_id) AS answer_count
     , LAG(COUNT(DISTINCT user_id),1,0) OVER (
          ORDER BY question ASC) AS previous_answer_count
    , ROUND(COUNT(DISTINCT user_id)*100.0/LAG(COUNT(DISTINCT user_id),1,0) OVER (
          ORDER BY question ASC),2) AS percentage_of_previous_answer
FROM survey
GROUP BY 1;
```

**Outcome:**

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/f6838f73-d10d-45d5-ad5c-6cbf76c70b9a)
- Lowest engagement on Question 5 "When was your last eye exam?". 25% of users didn't answer. Reasons behind this could be the uncertainty of how this information would be valuable in this survey/reluctance in sharing private information, an unsuitable choice of options or the disconnect with getting on with the survey after 4 questions answered.
  <br>→ **Actions**: Explaining why Warby Parker asks for this information, reassuring that no data will be sold for marketing use, A/B testing the option choices, A/B testing engagement of users with removing this last question
- Second lowest engagement on Question 3 "Which shapes do you like?". The shape of glasses might be the most uncertain factor in this buying glasses process, and therefore the first factor behind the home-try-on trial, or users might not want to discard any option in order to try various styles.
 <br>→ **Actions**: offering a multiple choice option, explaining that several shapes of glasses can be chosen afterwards, in the test process
- **Bonus Action** for Question 2, “What’s your fit?”. It has a high response rate (95%) but almost 1 out of 10 answers are uncertain. Could we add indication for users to be able to measure their fit?

## Home Try-On Funnel:
Warby Parker’s purchase funnel is: Take the Style Quiz → Home Try-On → Purchase the Perfect Pair of Glasses.
During the Home Try-On stage, we will be conducting an A/B Test: 50% of the users will get 3 pairs to try (control A) on and 50% will try on 5 pairs (variant B).

Let's merge data from these 3 tables and see this data from a user_id granularity:
```sql
SELECT DISTINCT q.user_id
              , h.user_id IS NOT NULL AS 'is_home_try_on',
              , h.number_of_pairs AS 'number_of_pairs',
              , p.user_id IS NOT NULL AS 'is_purchase',
              , p.price AS "revenue"
FROM quiz AS 'q'
LEFT JOIN home_try_on AS 'h' ON q.user_id = h.user_id
LEFT JOIN purchase AS 'p' ON h.user_id = p.user_id
```
*Top tips while creating this funnel:*
- Left joins are the most suitable solution there as we also want to see users who took the quiz but didn’t converted in a Home-Try-On test
- Renaming the new columns is important for the data to be easily readable


We now want to group our data by `number_of_pairs` and calculate conversion rates and more:
```sql
-- Let's create a funnel from the quiz to the home try-on trial of Warby Parker Glasses-
WITH funnel AS(
  SELECT DISTINCT q.user_id
                , h.user_id IS NOT NULL AS is_home_try_on
                , h.number_of_pairs AS number_of_pairs
                , p.user_id IS NOT NULL AS is_purchase
                , p.price AS revenue
  FROM quiz AS 'q'
LEFT JOIN home_try_on AS 'h'
ON q.user_id = h.user_id
LEFT JOIN purchase AS 'p'
ON h.user_id = p.user_id)
-- and now let's select the information needed for good visbility on how the 3/5 pairs A/B test has influenced the transactions and revenue
SELECT number_of_pairs 
     , SUM (is_home_try_on) AS home_tries
     , SUM (is_purchase) AS purchases
     , ROUND(SUM (is_purchase)*100.0/SUM (is_home_try_on),2) AS conversion_rate
     , SUM (revenue) AS test_revenue
     , SUM(revenue)/SUM (is_purchase) AS average_revenue_per_purchase
FROM funnel
WHERE number_of_pairs IS NOT NULL
GROUP BY number_of_pairs;
```

**Outcome:**

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/33602dcd-285c-4d03-9547-7cc4358d3289)

- CVR is much higher for variant group. Users receiving 5 pairs are +33pp more likely to complete a purchase than 3 pairs-testers control group. Only half of control group are converting vs 79% of variant group.
- The average revenue per purchase is similar for both user groups = £113
 <br>→ **Test conclusions**: The A/B test is successful for the variant group. Warby Parker would most likely experience an increase in purchases by moving 100% home-try-ons to 5 pairs tests.
