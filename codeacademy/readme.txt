Completed Code in Learning Environment

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt


species = pd.read_csv('species_info.csv')


species_count = species.scientific_name.nunique()
print species_count
print species.head()

species_type = species.category.unique()
print species_type

conservation_statuses = species.conservation_status.unique()
print conservation_statuses

conservation_counts = species.groupby('conservation_status').scientific_name.nunique().reset_index()
print conservation_counts

species.fillna('No Intervention', inplace = True)

conservation_counts_fixed = species.groupby('conservation_status').scientific_name.nunique().reset_index()
print conservation_counts

____________________________________________

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)

protection_counts = species.groupby('conservation_status')\
    .scientific_name.nunique().reset_index()\
    .sort_values(by='scientific_name')

plt.figure(figsize=(10,4))
ax = plt.subplot()

plt.bar(range(len(protection_counts.conservation_status)), protection_counts.scientific_name)

ax.set_xticks(range(len(protection_counts.conservation_status)))
ax.set_xticklabels(protection_counts.conservation_status)
ax.set_ylabel('Number of Species')
plt.title('Conservation Status by Species')
plt.show()

______________________________________________

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)

species['is_protected'] = species.conservation_status != 'No Intervention'

category_counts = species.groupby(['category', 'is_protected']).scientific_name.nunique().reset_index()

category_pivot = category_counts.pivot(columns='is_protected',
                      index='category',
                      values='scientific_name')\
                      .reset_index()
  
category_pivot.columns = ['category', 'not_protected', 'protected']

category_pivot['percent_protected'] = category_pivot.protected / (category_pivot.protected + category_pivot.not_protected)

print category_pivot
___________________________________________________

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt
from scipy.stats import chi2_contingency

contingency = [[30, 146],
              [75, 413]]

pval = chi2_contingency(contingency)[1]
print(pval)
# No significant difference because pval > 0.05

contingency_reptile_mammal = [[30, 146],
                              [5, 73]]

pval_reptile_mammal = chi2_contingency(contingency_reptile_mammal)[1]
print(pval_reptile_mammal)
# Significant difference! pval_reptile_mammal < 0.05

____________________________________________________________

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species.fillna('No Intervention', inplace = True)
species['is_protected'] = species.conservation_status != 'No Intervention'

observations = pd.read_csv('observations.csv')
print observations.head()

is_sheep = lambda species: 'Sheep' in species.common_names

species['is_sheep'] = species.apply(is_sheep, axis=1)

species_is_sheep = species[(species.is_sheep == True)]

print species.head()

print species_is_sheep

sheep_species = species[(species.is_sheep == True) & (species.category == 'Mammal')]

print sheep_species

sheep_observations = pd.merge(sheep_species, observations)

print sheep_observations.head()

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()

print obs_by_park

__________________________________________________

import codecademylib
import pandas as pd
from matplotlib import pyplot as plt

species = pd.read_csv('species_info.csv')
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]

observations = pd.read_csv('observations.csv')

sheep_observations = observations.merge(sheep_species)

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()

plt.figure(figsize=(16,4))
ax = plt.subplot()

plt.bar(range(len(obs_by_park.observations)), obs_by_park.observations)

ax.set_xticks(range(len(obs_by_park.observations)))
ax.set_xticklabels(obs_by_park.park_name)
ax.set_ylabel('Number of Observations')
plt.title('Observations of Sheep per Week')
plt.show()

___________________________________________________

baseline = 15

minimum_detectable_effect = 100*5./15

sample_size_per_variant = 870

yellowstone_weeks_observing = sample_size_per_variant/507.

bryce_weeks_observing = sample_size_per_variant/250.

print minimum_detectable_effect
print yellowstone_weeks_observing
print bryce_weeks_observing
