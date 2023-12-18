# MuscleHub A/B Test
Help MuscleHub analyze an A/B test and choose a business strategy.

You’ve been hired to help MuscleHub, a fancy gym, run an A/B test!

Currently, when a visitor to MuscleHub is considering buying a membership, he or she follows the following steps:

Take a fitness test with a personal trainer
Fill out an application for the gym
Send in their payment for their first month’s membership
Janet, the manager of MuscleHub, thinks that the fitness test intimidates some prospective members, so she has set up an A/B test.

Visitors will randomly be assigned to one of two groups:

Group A will still be asked to take a fitness test with a personal trainer
Group B will skip the fitness test and proceed directly to the application
Janet’s hypothesis is that visitors assigned to Group B will be more likely to eventually purchase a membership to MuscleHub.

```python
from codecademySQL import sql_query

visits = sql_query('''
SELECT * FROM visits LIMIT 5 ''')
display(visits)
fitness_tests = sql_query('''
SELECT * FROM fitness_tests LIMIT 5 ''')
display(fitness_tests)
applications = sql_query(''' 
SELECT * FROM applications LIMIT 5 ''')
display(applications)
purchases = sql_query(''' 
SELECT * FROM purchases LIMIT 5 ''')
display(purchases)

df = sql_query(''' SELECT 
visits.first_name, 
visits.last_name, 
visits.gender, 
visits.visit_date,
fitness_tests.fitness_test_date,
applications.application_date,
purchases.purchase_date

FROM visits 

LEFT JOIN 

fitness_tests ON fitness_tests.first_name = visits.first_name
                AND fitness_tests.last_name = visits.last_name
                AND fitness_tests.email = visits.email

LEFT JOIN

applications ON applications.first_name = visits.first_name
                AND applications.last_name = visits.last_name
                AND applications.email = visits.email

LEFT JOIN 

purchases ON purchases.first_name = visits.first_name
            AND purchases.last_name = visits.last_name
            AND purchases.email = visits.email

WHERE visits.visit_date >= '7-1-17' ''')


print(len(df))

import pandas as pd
from matplotlib import pyplot as plt

df["ab_test_group"] = df["fitness_test_date"].apply(lambda x: "A" if pd.notnull(x) else "B")
ab_counts = df.groupby("ab_test_group").first_name.count().reset_index()
display(ab_counts)

labels_ab= ["A", "B"]
colors_ab = ["turquoise", "violet" ]

plt.pie(ab_counts.first_name, labels= labels_ab, autopct = "%0.2f%%", colors = colors_ab)
plt.axis("equal")
plt.legend(labels_ab)
plt.title("A/B Test Distribution")
plt.savefig("ab_test_pie_chart.png")

df["is_application"] = df["application_date"].apply(lambda x: "Application" if pd.notnull(x) else "No Application")

app_counts = df.groupby(["ab_test_group", "is_application"]).first_name.count().reset_index()
display(app_counts)

app_pivot = app_counts.pivot(columns="is_application",
                    index = "ab_test_group",
                    values = "first_name").reset_index()
display(app_pivot)

app_pivot["Total"] = app_pivot["Application"] + app_pivot["No Application"]
display(app_pivot["Total"])
app_pivot["Percent with Application"] = app_pivot["Application"] / app_pivot["Total"]
display(app_pivot)

from scipy.stats import chi2_contingency
contingency = ([250,2254], [325,2175])
chi2_contingency(contingency)

df["is_member"] = df["purchase_date"].apply(lambda x: "Member" if pd.notnull(x) else "Not Member")
just_apps = df[df.is_application == "Application"]

member_counts = just_apps.groupby(["ab_test_group", "is_member"]).first_name.count().reset_index()
display(member_counts)

member_pivot = member_counts.pivot(columns ="is_member",
                                  index="ab_test_group",
                                  values = "first_name").reset_index()
member_pivot["Total Member"] = member_pivot["Member"] + member_pivot["Not Member"]
member_pivot["Percent with Member"] = member_pivot["Member"] / member_pivot["Total Member"]
display(member_pivot)

contingency_app = ([200, 50], [250,75])
chi2_contingency(contingency_app)

final_member_count = df.groupby(["ab_test_group", "is_member"]).first_name.count().reset_index()
final_member_pivot = final_member_count.pivot(columns = "is_member",
                                               index = "ab_test_group",
                                               values= "first_name").reset_index()
final_member_pivot["Total"] = final_member_pivot["Member"] + final_member_pivot["Not Member"]
final_member_pivot["Percent Purchase"] = final_member_pivot["Member"] / final_member_pivot["Total"]
display(final_member_pivot)

contingency_visit = ([200,2304], [250,2250])
chi2_contingency(contingency_visit)

ax = plt.subplot()
plt.bar(range(len(app_pivot)), app_pivot["Percent with Application"].values, color=["turquoise", "violet"])

ax.set_xticks(range(len(app_pivot)))
ax.set_xticklabels(["Fitness Test", "No Fitness Test"])
ax.set_yticks([0, 0.05, 0.10, 0.15, 0.20])
ax.set_yticklabels(["%0", "%5", "%10", "%15", "%20"])
plt.title("Percent of Visitors Who Apply")
plt.show()

ax = plt.subplot()
plt.bar(range(len(member_pivot)), member_pivot["Percent with Member"], color=["turquoise", "violet"] )
ax.set_xticks(range(len(member_pivot)))
ax.set_xticklabels(["Fitness Test", "Not Fitness Test"])
ax.set_yticks([0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1])
ax.set_yticklabels(['0%', '10%', '20%', '30%', '40%', '50%', '60%', '70%', '80%', '90%', '100%'])
plt.title("Percent of Applicants Who Purchase a Membership")
plt.show()


ax = plt.subplot()
plt.bar(range(len(final_member_pivot)), final_member_pivot["Percent Purchase"], color=["turquoise", "violet"])
ax.set_xticks(range(len(final_member_pivot)))
ax.set_xticklabels(["Fitness Test", "Not Fitness Test"])
ax.set_yticks([0, 0.05, 0.10, 0.15, 0.20])
ax.set_yticklabels(["%0", "%5", "%10", "%15", "%20"])
plt.title("Percent of Visitors Who Purchase a Membership")
plt.show()
```