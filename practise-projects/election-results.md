# Election Results
You’re part of an impartial research group that conducts phone surveys prior to local elections. During this election season, the group conducted a survey to determine how many people would vote for Cynthia Ceballos vs. Justin Kerrigan in the mayoral election.
Now that the election has occurred, your group wants to compare the survey responses to the actual results.

```python
import codecademylib
import numpy as np
from matplotlib import pyplot as plt

survey_responses = ['Ceballos', 'Kerrigan', 'Ceballos', 'Ceballos', 'Ceballos','Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Ceballos',
'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Ceballos', 'Ceballos', 'Ceballos', 'Ceballos',
'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Ceballos',
'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Ceballos', 'Ceballos', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Kerrigan', 'Ceballos']


total_ceballos = sum([1 for response in survey_responses if response == "Ceballos" ])
print(total_ceballos)
survey_length= float(len(survey_responses))
percentage_ceballos = total_ceballos / survey_length
print(percentage_ceballos)
possible_surveys = np.random.binomial(survey_length, .54, 10000) / survey_length
plt.hist(possible_surveys, range = (0,1), bins=20)
plt.show()


ceballos_loss_surveys = np.mean(possible_surveys < 0.5)
ceballos_loss_surveys_percentage = ceballos_loss_surveys * 100
print(ceballos_loss_surveys_percentage)
large_survey_length = float(7000)
large_survey= np.random.binomial(large_survey_length, 0.54, size=10000)
print(large_survey) / large_survey_length
plt.show()


new_large_survey = np.random.binomial(large_survey_length, 0.54, size=10000)
ceballos_loss_new = np.mean(new_large_survey<0.5) * 100
print(ceballos_loss_new)
```