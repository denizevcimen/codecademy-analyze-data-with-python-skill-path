# CrunchieMunchies
You work in marketing for a food company YummyCorps, which is developing a new kind of tasty, wholesome cereal called CrunchieMunchies. You want to demonstrate to consumers how healthy your cereal is in comparison to other leading brands, so you’ve dug up nutritional data on several different competitors.
Your task is to use NumPy statistical calculations to analyze this data and prove that your CrunchieMunchies cereal is the healthiest choice for consumers.

```python
import codecademylib
import numpy as np
calorie_stats = np.genfromtxt("cereal.csv", delimiter=",")
print(calorie_stats)
average_calories = np.mean(calorie_stats)
print(average_calories)
calorie_stats_sorted = np.sort(calorie_stats)
print(calorie_stats_sorted)
median_calories = np.median(calorie_stats)
print(median_calories)
nth_percentile= np.percentile(calorie_stats, 4)
print(nth_percentile)
more_calories = np.mean(calorie_stats > 60) * 100
print(more_calories)
calorie_std = np.std(calorie_stats)
print(calorie_std)
print("%96.10 our cereal have less calorie than the others brand")
```