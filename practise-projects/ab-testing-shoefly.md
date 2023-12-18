# A/B Testing for ShoeFly.com
Our favorite online shoe store, ShoeFly.com is performing an A/B Test. They have two different versions of an ad, which they have placed in emails, as well as in banner ads on Facebook, Twitter, and Google. They want to know how the two ads are performing on each of the different platforms on each day of the week. Help them analyze the data using aggregate measures.

```python
import codecademylib3
import pandas as pd

ad_clicks = pd.read_csv('ad_clicks.csv')
print(ad_clicks.head())
ad_clicks.groupby("utm_source").user_id.count().reset_index()
ad_clicks["is_click"] = ~ad_clicks.ad_click_timestamp.isnull()

clicks_by_source = ad_clicks.groupby(["utm_source", "is_click"]).user_id.count().reset_index()
print(clicks_by_source)
clicks_pivot = clicks_by_source.pivot(
 columns = "is_click",
 index = "utm_source",
 values = "user_id"
).reset_index()
print(clicks_pivot)

clicks_pivot['percent_clicked'] = \
  clicks_pivot[True] / \
  (clicks_pivot[True] +
   clicks_pivot[False])
print(ad_clicks.groupby("experimental_group").user_id.count().reset_index()
)
print(ad_clicks.groupby(["experimental_group", "is_click"]).user_id.count().reset_index().pivot(
 columns = "is_click",
 index = "experimental_group",
 values = "user_id"
).reset_index()
)
a_clicks = ad_clicks[ad_clicks.experimental_group == "A"]
b_clicks = ad_clicks[ad_clicks.experimental_group == "B"]
a_clicks_pivot = a_clicks.groupby(["is_click", "day"]).user_id.count().reset_index().pivot(
 columns = "is_click",
 index = "day",
 values = "user_id"
).reset_index()

a_clicks_pivot["percent_clicked"] = a_clicks_pivot[True] / (a_clicks_pivot[True] + a_clicks_pivot[False])

b_clicks_pivot = b_clicks.groupby(["is_click", "day"]).user_id.count().reset_index().pivot(
 columns = "is_click",
 index = "day",
 values = "user_id"
).reset_index()

b_clicks_pivot["percent_clicked"] = b_clicks_pivot[True] / (b_clicks_pivot[True] + b_clicks_pivot[False])

print(a_clicks_pivot)
print(b_clicks_pivot)
```