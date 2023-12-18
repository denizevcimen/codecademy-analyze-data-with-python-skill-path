# Petal Power Inventory
You’re the lead data analyst for a chain of gardening stores called Petal Power. Help them analyze their inventory!

```python
import codecademylib3
import pandas as pd
inventory = pd.read_csv("inventory.csv")
inventory.head(10)
staten_island = inventory.iloc[0:10]
product_request = staten_island["product_description"]
print(inventory)
seed_request = inventory[(inventory.location == "Brooklyn") & (inventory.product_type == "seeds")]
inventory["in_stock"] = inventory.apply(lambda row: True if row.quantity >0 else False, axis=1)
inventory["total_value"] = inventory.price * inventory.quantity
combine_lambda = lambda row: \
   '{} - {}'.format(row.product_type,
                    row.product_description)


inventory["full_description"] = inventory.apply(combine_lambda, axis=1)
```