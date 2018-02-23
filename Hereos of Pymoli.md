

```python
import pandas as pd
import numpy as np
```


```python
purchase_data_df = pd.read_json("purchase_data.json")
purchase_data_df = purchase_data_df.rename(columns={'Item ID': 'Item_ID', 'Item Name': 'Item_Name'})
purchase_data_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item_ID</th>
      <th>Item_Name</th>
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
#Player Count
number_of_players = purchase_data_df.SN.nunique()
```


```python
total_players = {"Total Players": [number_of_players]}
total_players_df = pd.DataFrame(total_players)
total_players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Total)
```


```python
unique_items = purchase_data_df["Item_Name"].nunique()
```


```python
average_purchase_price = purchase_data_df.Price.mean()
```


```python
number_of_purchases = purchase_data_df.Age.count()
```


```python
total_revenue = purchase_data_df['Price'].sum()
```


```python
purchasing_analysis = {
    "Number of Unique Items": [unique_items], 
    "Average Purchase Price": [average_purchase_price], 
    "Total Number of Purchases": [number_of_purchases],
    "Total Revenue": [total_revenue]
        }
purchasing_analysis_df = pd.DataFrame(purchasing_analysis)
purchasing_analysis_df
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
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.931192</td>
      <td>179</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
```


```python
number_of_males = purchase_data_df.groupby(["Gender", "SN"]).SN.nunique().Male.sum()
number_of_females = purchase_data_df.groupby(["Gender", "SN"]).SN.nunique().Female.sum()
number_of_other_gender = purchase_data_df.groupby(["Gender", "SN"]).SN.nunique()['Other / Non-Disclosed'].sum()
```


```python
percent_males = number_of_males / number_of_players 
percent_females = number_of_females / number_of_players
percent_other = number_of_other_gender / number_of_players
```


```python
gender_demographics = {
    " ": ["Male", "Female", "Other"],
    "Percentage of Players": [percent_males, percent_females, percent_other],
    "Total Count": [number_of_males, number_of_females, number_of_other_gender],
}
gender_demographics_df = pd.DataFrame(gender_demographics)
gender_demographics_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>0.811518</td>
      <td>465</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>0.174520</td>
      <td>100</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other</td>
      <td>0.013962</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
```


```python
gender_df = purchase_data_df.groupby('Gender')
```


```python
male_purchase_count = gender_df.count().Age.Male
female_purchase_count = gender_df.count().Age.Female
other_purchase_count = gender_df.count().Age["Other / Non-Disclosed"]
```


```python
f_avg_purchase_price = gender_df.mean().Price.Female
m_avg_purchase_price = gender_df.mean().Price.Male
o_avg_purchase_price = gender_df.mean().Price["Other / Non-Disclosed"]
```


```python
f_total_purchase_value = gender_df.sum().Price.Female
m_total_purchase_value = gender_df.sum().Price.Male
o_total_purchase_value = gender_df.sum().Price["Other / Non-Disclosed"]
```


```python
purchasing_analysis = {
    "Gender": ["Female", "Male", "Other / Non-Disclosed"],
    "Purchase Count": [female_purchase_count, male_purchase_count, other_purchase_count],
    "Average Purchase Price": [f_avg_purchase_price, m_avg_purchase_price, o_avg_purchase_price],
    "Total Purchase Value": [f_total_purchase_value, m_total_purchase_value, o_total_purchase_value], 
   # "Normalized Totals": []
    # normalized totals ( x - mean ) / standard deviation
}
purchasing_analysis_df = pd.DataFrame(purchasing_analysis)
purchasing_analysis_df
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
      <th>Average Purchase Price</th>
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.815515</td>
      <td>Female</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.950521</td>
      <td>Male</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.249091</td>
      <td>Other / Non-Disclosed</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics
```


```python
np.arange(10,100,4)
age_bins = [0, 10, 14, 18, 22, 26, 30, 34, 38, 42, 46, 100]
```


```python
purchase_data_df['Age_Group'] = pd.cut(purchase_data_df['Age'], age_bins)
purchase_data_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item_ID</th>
      <th>Item_Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age_Group</th>
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
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>(30, 34]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>(22, 26]</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_groups = purchase_data_df.groupby('Age_Group')
```


```python
age_bins_list = age_groups.count().Age.index.tolist()
by_age_purchase_count = age_groups.count().Age.tolist()
by_age_average_purchase_price = age_groups.mean().Price.tolist()
by_age_total_purchase_value = age_groups.sum().Price.tolist()
```


```python
age_demographics = [('Age Groups', age_bins_list),
                    ('Purchase Count', by_age_purchase_count),
                    ('Average Purchase Price', by_age_average_purchase_price),
                    ('Total Purchase Value', by_age_total_purchase_value),
                   # ('Normalized Totals', np.arange(0,11, 1))
                          ]
age_demographics_df = pd.DataFrame.from_items(age_demographics)
age_demographics_df
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
      <th>Age Groups</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>(0, 10]</td>
      <td>32</td>
      <td>3.019375</td>
      <td>96.62</td>
    </tr>
    <tr>
      <th>1</th>
      <td>(10, 14]</td>
      <td>31</td>
      <td>2.702903</td>
      <td>83.79</td>
    </tr>
    <tr>
      <th>2</th>
      <td>(14, 18]</td>
      <td>111</td>
      <td>2.876757</td>
      <td>319.32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>(18, 22]</td>
      <td>231</td>
      <td>2.927273</td>
      <td>676.20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>(22, 26]</td>
      <td>207</td>
      <td>2.937295</td>
      <td>608.02</td>
    </tr>
    <tr>
      <th>5</th>
      <td>(26, 30]</td>
      <td>63</td>
      <td>2.983968</td>
      <td>187.99</td>
    </tr>
    <tr>
      <th>6</th>
      <td>(30, 34]</td>
      <td>46</td>
      <td>3.070435</td>
      <td>141.24</td>
    </tr>
    <tr>
      <th>7</th>
      <td>(34, 38]</td>
      <td>37</td>
      <td>2.812432</td>
      <td>104.06</td>
    </tr>
    <tr>
      <th>8</th>
      <td>(38, 42]</td>
      <td>20</td>
      <td>3.128000</td>
      <td>62.56</td>
    </tr>
    <tr>
      <th>9</th>
      <td>(42, 46]</td>
      <td>2</td>
      <td>3.265000</td>
      <td>6.53</td>
    </tr>
    <tr>
      <th>10</th>
      <td>(46, 100]</td>
      <td>0</td>
      <td>NaN</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders - 
# From here down it's largely unfinished.
# A lot of work has been done but it's not all put together yet
```


```python
sn_groups = purchase_data_df.groupby('SN')
sn_groups.sum().head()
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
      <th>Age</th>
      <th>Item_ID</th>
      <th>Price</th>
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
      <th>Adairialis76</th>
      <td>20</td>
      <td>44</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>Aduephos78</th>
      <td>111</td>
      <td>345</td>
      <td>6.70</td>
    </tr>
    <tr>
      <th>Aeduera68</th>
      <td>78</td>
      <td>301</td>
      <td>5.80</td>
    </tr>
    <tr>
      <th>Aela49</th>
      <td>25</td>
      <td>44</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>Aela59</th>
      <td>23</td>
      <td>63</td>
      <td>1.27</td>
    </tr>
  </tbody>
</table>
</div>




```python
top_5_total_purchase_value = sn_groups.sum().Price.nlargest(n=5)
top_5_total_purchase_value
```




    SN
    Undirrala66    17.06
    Saedue76       13.56
    Mindimnya67    12.74
    Haellysu29     12.73
    Eoda93         11.58
    Name: Price, dtype: float64




```python
top_5_total_purchase_value_list = top_5_total_purchase_value.index.tolist()
top_5_total_purchase_value_list
```




    ['Undirrala66', 'Saedue76', 'Mindimnya67', 'Haellysu29', 'Eoda93']




```python
top_5_total_purchase_value_df = purchase_data_df[purchase_data_df.SN.isin(top_5_total_purchase_value_list)].groupby("SN")
top_5_total_purchase_value_df
```




    <pandas.core.groupby.DataFrameGroupBy object at 0x1067f5a20>




```python
top_5_purchase_count = top_5_total_purchase_value_df.Price.count()
top_5_avg_price = top_5_total_purchase_value_df.Price.mean()
top_5_purchase_value = top_5_total_purchase_value_df.Price.sum()
top_5_purchase_count
```




    SN
    Eoda93         3
    Haellysu29     3
    Mindimnya67    4
    Saedue76       4
    Undirrala66    5
    Name: Price, dtype: int64




```python
item_name_groups = purchase_data_df.groupby("Item_Name")
item_name_list = item_name_groups.Item_Name.count().nlargest(n=5).index.tolist()
item_name_list
```




    ['Final Critic',
     'Arcane Gem',
     'Betrayal, Whisper of Grieving Widows',
     'Stormcaller',
     'Retribution Axe']




```python
popular_items_df = purchase_data_df[purchase_data_df.Item_Name.isin(item_name_list)]
popular_items_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item_ID</th>
      <th>Item_Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age_Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>54</th>
      <td>25</td>
      <td>Female</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Minduli80</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>57</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alallo58</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>61</th>
      <td>24</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Rinallorap73</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>62</th>
      <td>19</td>
      <td>Female</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Aeri84</td>
      <td>(18, 22]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#purchase count
pop_items_purchase_count_list = popular_items_df.Item_Name.value_counts()
pop_items_purchase_count_list
```




    Final Critic                            14
    Arcane Gem                              11
    Betrayal, Whisper of Grieving Widows    11
    Stormcaller                             10
    Retribution Axe                          9
    Name: Item_Name, dtype: int64




```python
# total purchase value
popular_items_df
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item_ID</th>
      <th>Item_Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age_Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>54</th>
      <td>25</td>
      <td>Female</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Minduli80</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>57</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alallo58</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>61</th>
      <td>24</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Rinallorap73</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>62</th>
      <td>19</td>
      <td>Female</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Aeri84</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>101</th>
      <td>25</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Assistasda90</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>116</th>
      <td>25</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Koikirra25</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>119</th>
      <td>19</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Yasur35</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>126</th>
      <td>25</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Raesty92</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>154</th>
      <td>27</td>
      <td>Female</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Lassilsa41</td>
      <td>(26, 30]</td>
    </tr>
    <tr>
      <th>171</th>
      <td>21</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Stryanastip77</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>175</th>
      <td>35</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Raillydeu47</td>
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>193</th>
      <td>19</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Farusrian86</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>194</th>
      <td>24</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Undadarla37</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>199</th>
      <td>30</td>
      <td>Female</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Phadai31</td>
      <td>(26, 30]</td>
    </tr>
    <tr>
      <th>226</th>
      <td>25</td>
      <td>Female</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Chamistast30</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>297</th>
      <td>24</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Yarolwen77</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>307</th>
      <td>26</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Aidain51</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>333</th>
      <td>22</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Aeliru63</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>338</th>
      <td>17</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Lisossanya98</td>
      <td>(14, 18]</td>
    </tr>
    <tr>
      <th>354</th>
      <td>20</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Mindirra92</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>361</th>
      <td>14</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Aeral97</td>
      <td>(10, 14]</td>
    </tr>
    <tr>
      <th>362</th>
      <td>36</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Lisosiast26</td>
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>380</th>
      <td>22</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Alaephos75</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>382</th>
      <td>25</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Yarithphos28</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>389</th>
      <td>37</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Iskistasda86</td>
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>406</th>
      <td>24</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Chamosia29</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>413</th>
      <td>24</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Sondadar26</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>415</th>
      <td>25</td>
      <td>Female</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Chamistast30</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>416</th>
      <td>25</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Hiarideu73</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>431</th>
      <td>37</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Aduephos78</td>
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>433</th>
      <td>23</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Ithesuphos68</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>440</th>
      <td>21</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Stryanastip77</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>442</th>
      <td>7</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Liri91</td>
      <td>(0, 10]</td>
    </tr>
    <tr>
      <th>448</th>
      <td>25</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Sausosia74</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>458</th>
      <td>25</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Jiskassa76</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>506</th>
      <td>29</td>
      <td>Male</td>
      <td>180</td>
      <td>Stormcaller</td>
      <td>2.78</td>
      <td>Tyalaesu89</td>
      <td>(26, 30]</td>
    </tr>
    <tr>
      <th>509</th>
      <td>28</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Iskista88</td>
      <td>(26, 30]</td>
    </tr>
    <tr>
      <th>535</th>
      <td>23</td>
      <td>Female</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Undirra73</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>541</th>
      <td>20</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Eosursurap97</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>554</th>
      <td>7</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Lisistaya47</td>
      <td>(0, 10]</td>
    </tr>
    <tr>
      <th>557</th>
      <td>24</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Iskimnya76</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>572</th>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Lisossa63</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>574</th>
      <td>7</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Eullydru35</td>
      <td>(0, 10]</td>
    </tr>
    <tr>
      <th>595</th>
      <td>20</td>
      <td>Male</td>
      <td>30</td>
      <td>Stormcaller</td>
      <td>4.15</td>
      <td>Eusur90</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>606</th>
      <td>20</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Lirtossa84</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>612</th>
      <td>18</td>
      <td>Female</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Isurria36</td>
      <td>(14, 18]</td>
    </tr>
    <tr>
      <th>648</th>
      <td>23</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Rithe53</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>663</th>
      <td>38</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Tyisriphos58</td>
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>711</th>
      <td>22</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Tyananurgue44</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>721</th>
      <td>26</td>
      <td>Male</td>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>2.35</td>
      <td>Aeduera68</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>737</th>
      <td>22</td>
      <td>Male</td>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>Sondim68</td>
      <td>(18, 22]</td>
    </tr>
    <tr>
      <th>742</th>
      <td>26</td>
      <td>Male</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Inguron55</td>
      <td>(22, 26]</td>
    </tr>
    <tr>
      <th>746</th>
      <td>35</td>
      <td>Male</td>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>4.14</td>
      <td>Ralasti48</td>
      <td>(34, 38]</td>
    </tr>
    <tr>
      <th>766</th>
      <td>22</td>
      <td>Female</td>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>2.23</td>
      <td>Nitherian58</td>
      <td>(18, 22]</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
```


```python
# 5 most profitable items
most_profitable_items_list = purchase_data_df.groupby("Item_Name").sum().Price.nlargest(n=5).index.tolist()
most_profitable_items_list
profti

```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-38-8456a5facf1e> in <module>()
          2 most_profitable_items_list = purchase_data_df.groupby("Item_Name").sum().Price.nlargest(n=5).index.tolist()
          3 most_profitable_items_list
    ----> 4 profti
    

    NameError: name 'profti' is not defined



```python
# Item ID
```


```python
#purchase count
every_pop_item_purchase = purchase_data_df[purchase_data_df.Item_Name.isin(most_profitable_items_list)]
every_pop_item_purchase.groupby("Item_Name").count()
```


```python
#item price
purchase_data_df[purchase_data_df.Item_Name.isin(most_profitable_items_list)]
```
