## 🛋 Lovely Loveseats : Programming, Calculations

## Objective and Context
A new Furniture Store just opened "Lovely Loveseats". We want to support the speed up the process of creating receipts for customers.

***Tools:*** The full .py script is [here](https://github.com/elizabeth-gj/Codecademy-Projects/blob/master/Lovely-Loveseats-Programming-Calculations/lovely_loveseats_py)

## Building the Store Catalog:

Let's start building Lovely Loveseats catalog with 3 different items and their attached price.
```py
lovely_loveseat_description = '''Lovely Loveseat. Tufted polyester blend on wood. 32 inches high x 40 inches wide x 30 inches deep. Red or white.'''
lovely_loveseat_price = 254

stylish_settee_description = '''Stylish Settee. Faux leather on birch. 29.50 inches high x 54.75 inches wide x 28 inches deep. Black.'''
stylish_settee_price = 180.5

luxurious_lamp_description = '''Luxurious Lamp. Glass and iron. 36 inches tall. Brown with cream shade.'''
luxurious_lamp_price = 52.15
```

We will also need to have some sales tax reference in order to calculate sales revenue. This will be set up at 8.8%:

```py
sales_tax = 0.88
```

## The First Customer Enters the Store:
We want to be able to track a customer's total spend and the items they're purchasing. Let's create two variablees:

```py
customer_one_total = 0
customer_one_itemization = ''
```

This customer has loved our Seat and Lamp. Let's add modify our variables - and let's not forget the tax amount. What would their receipt look like?
```py
customer_one_total += lovely_loveseat_price
customer_one_itemization += lovely_loveseat_description

customer_one_total += luxurious_lamp_price
customer_one_itemization += luxurious_lamp_description

customer_one_tax = customer_one_total * sales_tax
customer_one_total += customer_one_tax

print ('Customer one items :')
print (customer_one_itemization)
print ('Customer one total :')
print (customer_one_total)
```
Here below is our receipt for Customer one:

![image](https://github.com/elizabeth-gj/Codecademy-Projects/assets/64903268/35af0c01-4ae0-455e-a0a9-e4b77b355bef)


