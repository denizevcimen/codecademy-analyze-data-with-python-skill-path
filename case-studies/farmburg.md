# Getting Familiar with FarmBurg
Brian is a Product Manager at FarmBurg, a company that makes a farming simulation social network game. In the FarmBurg game, you can plow, plant, and harvest different crops.
Today, you will be acting as Brian’s data analyst for an A/B Test that he has been conducting.

```python
import codecademylib
import pandas as pd
df = pd.read_csv("clicks.csv")
print(df.head())
df["is_purchase"] = df.click_day.apply(lambda x: "Purchase" if pd.notnull(x) else "No Purchase")
purchase_counts = df.groupby(["group", "is_purchase"]).user_id.count().reset_index()
print(purchase_counts)

from scipy.stats import chi2_contingency
contingency = [[316, 1350],
              [183, 1483],
              [83, 1583]]
chi2_stat, pvalue, dof, expected = chi2_contingency(contingency)
print pvalue
is_significant = True
num_visits = len(df)
p_clicks_099 = (1000 / 0.99) / num_visits
p_clicks_199 = (1000 / 1.99) / num_visits
p_clicks_499 =(1000 / 4.99) / num_visits
p_clicks_099 = (1000 / 0.99) / num_visits
p_clicks_199 = (1000 / 1.99) / num_visits
p_clicks_499 = (1000 / 4.99) / num_visits

pvalueA = binom_test(316, 1666, p_clicks_099)

pvalueB = binom_test(183, 1666, p_clicks_199)

pvalueC = binom_test(83, 1666, p_clicks_499)

print(pvalueA)
print(pvalueB)
print(pvalueC)

final_answer = 4.99
```