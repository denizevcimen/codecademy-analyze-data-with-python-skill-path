# Familiar: A Study In Data Analysis
Welcome to Familiar, a startup in the new market of blood transfusion! You’ve joined the team because you appreciate the flexible hours and extremely intelligent team, but the overeager doorman welcoming you into the office is a nice way to start your workday (well, work-evening).
Familiar has fallen into some tough times lately, so you’re hoping to help them make some insights about their product and help move the needle (so to speak).

```python
import familiar
vein_pack_lifespan = familiar.lifespans(package="vein")
from scipy.stats import ttest_1samp
vein_pack_test = ttest_1samp(vein_pack_lifespan, 71)
print vein_pack_test.pvalue


if vein_pack_test.pvalue <0.05:
  print("The Vein Pack Is Proven To Make You Live Longer!")
else:
   print("The Vein Pack Is Probably Good For You Somehow!")


artery_pack_lifespans = familiar.lifespans(package="artery")


from scipy.stats import ttest_ind


package_comparison_results = ttest_ind(vein_pack_lifespan,artery_pack_lifespans)


if package_comparison_results.pvalue <0.05:
 print("the Artery Package guarantees even stronger results!")
else:
 print("the Artery Package is also a great product!")


from scipy.stats import chi2_contingency


iron_contingency_table = familiar.iron_counts_for_package()
print iron_contingency_table


chi2_stat, iron_pvalue, dof, expected = chi2_contingency(iron_contingency_table)
 if iron_pvalue <0.05:
 print("The Artery Package Is Proven To Make You Healthier!")
else:
 print("While We Can't Say The Artery Package Will Help You, I Bet It's Nice!")
```