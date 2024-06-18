## ✂ Carly's Clippers : Loops & Lists

## Objective and Context
Carly's Clippers is a hair salon. We're provided with data that has been collected over the past week. Carly needs us to plan out the business operation for the rest of the month. What can we help her with?

3 lists have been provided:
- `hairstyles` : all the cuts offered at the salon
- `prices` : prices of each hairstyle in prior list
- `last_week` : the number of purchases each hairstule got in the last week
Each index in hairstyles corresponds to an associated index in prices and last_week.

```py
hairstyles = ["bouffant", "pixie", "dreadlocks", "crew", "bowl", "bob", "mohawk", "flattop"]
prices = [30, 25, 40, 20, 20, 35, 50, 35]
last_week = [2, 3, 5, 8, 4, 4, 6, 2]
```

***Tools:*** The full .py script is [here]()

## Prices:

Let's have a look at the average price per haircut with the provided data:

```py
total_price = 0
for p in prices:
  total_price = total_price + p
average_price = total_price / len(prices)
print("Average Haircut Price: "+str(average_price))
```

**Outcome:**
The average haircut price is $31.9 - which seem a bit expensive. Let's try and see what new discounted prices can be - maybe it will help attract a new clientele. We're discounting each cut by $5.

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/8a36850f-3860-470a-b703-841e9b0eaafd)

```py
new_prices = [p -5 for p in prices]
print ("New Prices :" + str(new_prices))
```
![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/7ed8ff4c-b182-4244-a744-a72a4271e206)

## Revenue:

How much Revenue has Carly's Clippers generated over the last week?

```py
total_revenue = 0
for i in range(len(hairstyles)):
  total_revenue += prices[i] * last_week[i]
print("Total Revenue: " + str(total_revenue))
```

We're using **loops** here to run through all of `last_week`'s purchases x the attached haircut price.

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/afcd23ee-5821-48b5-95e1-a0303047cef2)

A total of $1,085 in Revenue has been generated last week! 

```py
average_daily_revenue = total_revenue / 7
print("Average Daily Revenue: " + str(average_daily_revenue))
```

This makes an Average Daily Revenue of $155:

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/866a0ea4-c965-45d0-b716-9bf743281039)

We think a good tactic to grow our future revenue is to bring on more customers on the haircut that are on the lower end of the price range. We need to tell Carly what these are:
```py
cuts_under_30 = [hairstyles[i] for i in range(len(hairstyles)) if new_prices[i] <30]
print("Cuts Under 30: " + str(cuts_under_30))
```
![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/750f51dc-301e-40a2-803a-bf8b41286b2d)
