# Heroes Of Pymoli Data Analysis

* Of 780 purchasers, the largest plurality (336 or 43%) fall into 20-24 age bracket and they generate the most aggregate revenue ($978.77). However, these users are the least lucrative on a per user basis ($3.78). Older users are more lucrative on a per user basis (at least $4.20 per user for all cohorts 25+), however the size of the entire age 25+ popular (258) is only 75% the size of the 20-24 age bracket alone.

* The highest selling items in volume tend to be the cheaper items. Of the top 5 by sales, only the Retribution Axe ($4.45) costs more than $2.50.

* At this point, 'whales' in the game are still relatively small fish. Of the big spenders, only one even exceeds $15 thus far.




```python
# import dependencies
import pandas as pd
```


```python
#import file
purchase_data = pd.read_json('purchase_data.json')

purchase_data.to_csv('test.csv')
```


```python
#Define convert_to_currency for use in mapping
def convert_to_currency(number):
    return '${:,.2f}'.format(number)
```


```python
#overview data
purchase_data.info()
purchase_data.head()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 780 entries, 0 to 779
    Data columns (total 6 columns):
    Age          780 non-null int64
    Gender       780 non-null object
    Item ID      780 non-null int64
    Item Name    780 non-null object
    Price        780 non-null float64
    SN           780 non-null object
    dtypes: float64(1), int64(2), object(3)
    memory usage: 42.7+ KB





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#player count

user_count = purchase_data['SN'].nunique()

print('There are ' + str(user_count) + ' unique players.')
```

    There are 573 unique players.



```python
### Purchasing Analysis (Total)

#Number of Unique Items
item_count = purchase_data['Item ID'].nunique()

#Average Purchase Price
#Total Number of Purchases
#Total Revenue

total_revenue = purchase_data['Price'].sum()

purchase_count = len(purchase_data)

average_purchase = total_revenue / purchase_count

purchase_dict = [{'Number of Unique Items': item_count,
                       'Average Price': average_purchase,
                       'Number of Purchases': purchase_count,
                       'Total Revenue': total_revenue}]

purchase = pd.DataFrame(data=purchase_dict)

#Re-ordering the column, for some reason the function above puts them in arbitrary order
purchase = purchase[['Number of Unique Items', 
                                                 'Average Price', 
                                                 'Number of Purchases', 
                                                 'Total Revenue']]

purchase['Average Price'] = purchase['Average Price'].map(convert_to_currency)
purchase['Total Revenue'] = purchase['Total Revenue'].map(convert_to_currency)

purchase
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Gender Demographics
   #Percentage and Count of Male Players
   #Percentage and Count of Female Players
   #Percentage and Count of Other / Non-Disclosed

gender_data = purchase_data.set_index('Gender')

total_count = len(gender_data)
male_count = len(gender_data.loc['Male'])
female_count = len(gender_data.loc['Female'])
other_count = len(gender_data.loc['Other / Non-Disclosed'])

gender_dict = {'Gender' : ['Male', 'Female', 'Other / Nondisclosed'],
               'Percentage of Players': [male_count/total_count, female_count/total_count, other_count/total_count],
               'Player Count': [male_count, female_count, other_count]
              }

gender = pd.DataFrame(gender_dict)

gender['Percentage of Players'] = round(gender['Percentage of Players']*100, 2)

gender = gender.set_index('Gender')

gender

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Player Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.44</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Other / Nondisclosed</th>
      <td>1.41</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Age Demographics
   #Percentage and Count of All Age Bins
    
bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]

bin_names = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']

purchase_data['Age Bracket'] = pd.cut(purchase_data['Age'], bins, labels=bin_names)


agebin = purchase_data.set_index('Age Bracket')


total_count = len(agebin)
count_less10 = len(agebin.loc['<10'])
count_10to14 = len(agebin.loc['10-14'])
count_15to19 = len(agebin.loc['15-19'])
count_20to24 = len(agebin.loc['20-24'])
count_25to29 = len(agebin.loc['25-29'])
count_30to34 = len(agebin.loc['30-34'])
count_35to39 = len(agebin.loc['35-39'])
count_40plus = len(agebin.loc['40+'])


agebin_dict = {'Age Brackets' : ['<10', 
                                 '10-14', 
                                 '15-19', 
                                 '20-24', 
                                 '25-29', 
                                 '30-34', 
                                 '35-39', 
                                 '40+'],
               'Percentage of Players': [count_less10 / total_count,
                                        count_10to14 / total_count,
                                        count_15to19 / total_count,
                                        count_20to24 / total_count,
                                        count_25to29 / total_count,
                                        count_30to34 / total_count,
                                        count_35to39 / total_count,
                                        count_40plus / total_count],
               'Player Count': [count_less10,
                                count_10to14,
                                count_15to19,
                                count_20to24,
                                count_25to29,
                                count_30to34,
                                count_35to39,
                                count_40plus]
              }

age = pd.DataFrame(agebin_dict)

age['Percentage of Players'] = round(age['Percentage of Players'] * 100, 2)

age


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age Brackets</th>
      <th>Percentage of Players</th>
      <th>Player Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;10</td>
      <td>3.59</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10-14</td>
      <td>4.49</td>
      <td>35</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-19</td>
      <td>17.05</td>
      <td>133</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20-24</td>
      <td>43.08</td>
      <td>336</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-29</td>
      <td>16.03</td>
      <td>125</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30-34</td>
      <td>8.21</td>
      <td>64</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35-39</td>
      <td>5.38</td>
      <td>42</td>
    </tr>
    <tr>
      <th>7</th>
      <td>40+</td>
      <td>2.18</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Purchasing Analysis (Gender)

#The below each broken by gender
  # Purchase Count
  # Average Purchase Price
  # Total Purchase Value
  # Normalized Totals

group_gender = purchase_data.groupby('Gender')

gender_purchase = group_gender.agg({'Item ID': 'count',
                          'Price': ['mean', 'sum'],
                          'SN': pd.Series.nunique
                          })

#rename columns, flattened from multi-index to single index
gender_purchase.columns = ['Purchase Count', 
                           'Average Price', 
                           'Total Purchase Value', 
                           'Unique User Count']

gender_purchase['Normalized Per User Price'] = gender_purchase['Total Purchase Value'] / gender_purchase['Unique User Count']

#remove 'Unique User Count', only used for calculation above
del gender_purchase['Unique User Count']

gender_purchase['Average Price'] = gender_purchase['Average Price'].map(convert_to_currency)
gender_purchase['Total Purchase Value'] = gender_purchase['Total Purchase Value'].map(convert_to_currency)
gender_purchase['Normalized Per User Price'] = gender_purchase['Normalized Per User Price'].map(convert_to_currency)


gender_purchase
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Per User Price</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Purchasing Analysis (Age)

# The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  # Purchase Count
  # Average Purchase Price
  # Total Purchase Value
  # Normalized Totals

group_agebin = purchase_data.groupby('Age Bracket')

age_purchase = group_agebin.agg({'Item ID': 'count',
                        'Price': ['mean', 'sum'],
                        'SN': pd.Series.nunique
                        })

age_purchase.columns = ['Purchase Count', 
                'Average Price', 
                'Total Purchase Value', 
                'Unique User Count']

age_purchase['Normalized Per User Price'] = age_purchase['Total Purchase Value'] / age_purchase['Unique User Count']

del age_purchase['Unique User Count']

age_purchase['Average Price'] = age_purchase['Average Price'].map(convert_to_currency)
age_purchase['Total Purchase Value'] = age_purchase['Total Purchase Value'].map(convert_to_currency)
age_purchase['Normalized Per User Price'] = age_purchase['Normalized Per User Price'].map(convert_to_currency)

age_purchase
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Per User Price</th>
    </tr>
    <tr>
      <th>Age Bracket</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Top Spenders

# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
  # SN
  # Purchase Count
  # Average Purchase Price
  # Total Purchase Value

group_sn = purchase_data.groupby('SN')

#aggregate relevant data
spenders = group_sn.agg({'Item ID': 'count',
             'Price': ['mean', 'sum']})

#rename columns
spenders.columns = ['Purchase Count', 'Average Purchase Price', 'Total Purchase Value']

#isolate top 5 spenders
top_5_spenders = spenders.nlargest(5, 'Total Purchase Value')

top_5_spenders['Average Purchase Price'] = top_5_spenders['Average Purchase Price'].map(convert_to_currency)
top_5_spenders['Total Purchase Value'] = top_5_spenders['Total Purchase Value'].map(convert_to_currency)


top_5_spenders

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Most Popular Items

# Identify the 5 most popular items by purchase count, then list (in a table):
  # Item ID
  # Item Name
  # Purchase Count
  # Item Price
  # Total Purchase Value

group_item = purchase_data.groupby(['Item ID', 'Item Name'])

items = group_item.agg({'Item ID': 'count',
             'Price': ['mean', 'sum']})

items.columns = ['Purchase Count', 'Item Price', 'Total Purchase Price']
items = items.sort_values(by=['Purchase Count', 'Total Purchase Price'], ascending=False)

top_5_items = items.nlargest(5, 'Purchase Count')

top_5_items['Item Price']= top_5_items['Item Price'].map(convert_to_currency)
top_5_items['Total Purchase Price'] = top_5_items['Total Purchase Price'].map(convert_to_currency)

top_5_items
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Price</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
### Most Profitable Items

# Identify the 5 most profitable items by total purchase value, then list (in a table):
  # Item ID
  # Item Name
  # Purchase Count
  # Item Price
  # Total Purchase Value

top_5_items_profit = items.nlargest(5, 'Total Purchase Price')

top_5_items_profit['Item Price']= top_5_items_profit['Item Price'].map(convert_to_currency)
top_5_items_profit['Total Purchase Price'] = top_5_items_profit['Total Purchase Price'].map(convert_to_currency)

top_5_items_profit
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Price</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


