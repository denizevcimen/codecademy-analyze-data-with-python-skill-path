# Betty's Bakery
Betty has always used her grandmother’s recipe book to make cookies, cakes, pancakes, and bread for her friends and family. She’s getting ready to open a business and will need to start buying all of her milk, eggs, sugar, flour, and butter in bulk.
Help Betty figure out how much she needs to buy using NumPy arrays describing her recipes.

```python
import numpy as np
cupcakes = np.array([2, 0.75, 2, 1, 0.5])
recipes = np.genfromtxt("recipes.csv", delimiter=",")
print(recipes)
eggs = recipes[:, 2]
one_egg = recipes[(eggs==1)]
print(one_egg)
cookies = recipes[2, :]
double_batch = cupcakes * 2
grocery_list = cookies + double_batch
print(grocery_list)
```