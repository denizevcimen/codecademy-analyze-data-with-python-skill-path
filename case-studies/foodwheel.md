# FoodWheel: Let the Food Choose For You
FoodWheel is a startup delivery service that takes away the struggle of deciding where to eat! FoodWheel picks you an amazing local restaurant and lets you order through the app. Senior leadership is getting ready for a big board meeting, and as the resident Data Analyst, you have been enlisted to help decipher data and create a presentation to answer several key questions:
What cuisines does FoodWheel offer? Which areas should the company search for more restaurants to partner with?
How has the average order amount changed over time? What does this say about the trajectory of the company?
How much has each customer on FoodWheel spent over the past six months? What can this tell us about the average FoodWheel customer?

```python
from matplotlib import pyplot as plt
import pandas as pd
restaurants = pd.read_csv("restaurants.csv")
print(restaurants.head())
cuisine_options_count = restaurants.cuisine.nunique()
cuisine_counts = restaurants.groupby("cuisine").name.count().reset_index()
print(cuisine_counts)
cuisines = cuisine_counts.cuisine.values
counts = cuisine_counts.name.values

plt.pie(counts, labels=cuisines,autopct="%d%%")
plt.axis("equal")
plt.title("FoodWheel")
plt.show()
orders = pd.read_csv("orders.csv")
print(orders.head())

orders["month"] = orders.date.apply(lambda x: x.split("-")[0])
avg_order = orders.groupby("month").price.mean().reset_index()
print(avg_order)
std_order = orders.groupby("month").price.std().reset_index()

ax = plt.subplot()
bar_heights = avg_order.price
bar_errors = std_order.price
plt.bar(range(len(bar_heights)), bar_heights, yerr = bar_errors, capsize=5)
ax.set_xticks(range(len(bar_heights)))
ax.set_xticklabels(['April', 'May', 'June', 'July', 'August', 'September'])
plt.ylabel('Average Order Amount')
plt.title('Order Amount over Time')
plt.show()

customer_amount = orders.groupby("customer_id").price.sum().reset_index()
print(customer_amount.head())

plt.hist(customer_amount.price.values, range=(0,200), bins=40)
plt.xlabel("Total Spent")
plt.ylabel("Number of Customers")
plt.title("Customer Expenditure Over 6 Months")
plt.show()
```