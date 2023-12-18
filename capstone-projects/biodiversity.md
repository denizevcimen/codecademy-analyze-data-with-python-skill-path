# Biodiversity Project
Welcome to the Introduction to Data Analysis Biodiversity Capstone project!
You are a biodiversity analyst working for the National Parks Service. You have been given a CSV file species_info.csv with data about different species in our National Parks, including:
- The scientific name of each species
- The common names of each species
- The species conservation status

(CSV data based on data from the National Parks Service.)
The National Parks Service would like you to perform some data analysis on the conservation statuses of these species and to investigate if there are any patterns or themes to the types of species that become endangered. During this project, you will analyze, clean up, and plot data, pose questions and seek to answer them in a meaning way.


```python
import codecademylib3
import pandas as pd
import matplotlib as plt
species = pd.read_csv("species_info.csv")
print(species.head())
species_count = species.scientific_name.nunique()
species_type = species.category.unique()
conservation_statuses = species.conservation_status.unique()
conservation_counts = species.groupby("conservation_status").scientific_name.nunique().reset_index()
print(conservation_counts)
species.fillna('No Intervention', inplace = True)
conservation_counts_fixed = species.groupby("conservation_status").scientific_name.nunique().reset_index()
print(conservation_counts_fixed)

protection_counts = species.groupby('conservation_status')\
   .scientific_name.nunique().reset_index()\
   .sort_values(by='scientific_name')


plt.figure(figsize=(10,4))
ax = plt.subplot()
plt.bar(range(len(protection_counts)), protection_counts.scientific_name.values)
ax.set_xticks(range(len(protection_counts)))
ax.set_xticklabels(protection_counts.conservation_status.values)
plt.ylabel("Number of Species")
plt.title("Conservation Status by Species")
plt.show()
species['is_protected'] = species.conservation_status != 'No Intervention'

category_counts = species.groupby(["category", "is_protected"]).scientific_name.nunique().reset_index()

print(category_counts.head())
category_pivot = category_counts.pivot(columns="is_protected", index = "category",
values = "scientific_name").reset_index()
print(category_pivot)

category_pivot.columns = ['category', 'not_protected', 'protected']
category_pivot["percent_protected"] = category_pivot.protected / (category_pivot.protected + category_pivot.not_protected)
print(category_pivot)

contingency = [[30, 146],
             [75, 413]]


pval = chi2_contingency(contingency)[1]
print(pval)

contingency_reptile_mammal = [[30, 146],
                             [5, 73]]


pval_reptile_mammal = chi2_contingency(contingency_reptile_mammal)[1]
print(pval_reptile_mammal)
observations = pd.read_csv("observations.csv")
print(observations.head())

species["is_sheep"] = species.common_names.apply(lambda x: "Sheep" in x)
species_is_sheep = species[species.is_sheep]
print(species_is_sheep.head())
sheep_species = species[(species.is_sheep) & (species.category == "Mammal")]
print(sheep_species)

sheep_observations = sheep_species.merge(observations)


print(sheep_observations.head())
obs_by_park = sheep_observations.groupby("park_name").observations.sum().reset_index()

print(obs_by_park)
plt.figure(figsize=(16,4))
ax = plt.subplot()
plt.bar(range(len(obs_by_park)), obs_by_park.observations.values)
ax.set_xticks(range(len(obs_by_park)))
ax.set_xticklabels(obs_by_park.park_name.values)
plt.ylabel("Number of Observations")
plt.title("Observations of Sheep per Week")
plt.show()
baseline = 15
minimum_detectable_effect = 0.05 / 0.15 * 100
print(minimum_detectable_effect)
sample_size_per_variant = 870

yellowstone_weeks_observing = sample_size_per_variant / 507.

bryce_weeks_observing = 870 / 250.
```