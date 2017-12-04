

```python
# Heroes of PyMoli analysis and trends

# There are more male players than female and male spend more than females
# People in 19-22 age group spend the most on games
# Betrayal, Whisper of Grieving Widows is the most popular game; Retribution Axe is the most profitable game
```


```python
# Dependencies 
import pandas as pd
```


```python
# load JSON
purchase_data = "purchase_data.json"
```


```python
# Read with pandas
purchase_data_pd = pd.read_json(purchase_data)

```


```python
#Find total unique players
unique_players=purchase_data_pd["SN"].value_counts()
total_players_count = unique_players.count()
total_players_count
print ("Player Count")

total_players_count_df = pd.DataFrame({"Total Players": [total_players_count]
})
total_players_count_df
```

    Player Count





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
#Purchasing Analysis
purchasing_analysis = pd.DataFrame({"Number of Unique Items": 
                                    [len(purchase_data_pd.groupby('Item Name').agg({'Price':['count']}))],
                                    "Average Price":[purchase_data_pd["Price"].mean()],
                                    "Number of Purchases":[len(purchase_data_pd)],
                                    "Total Revenue":[purchase_data_pd["Price"].sum()]
})

print("Purchasing Anlayis (Total)")

purchasing_analysis["Total Revenue"] = purchasing_analysis["Total Revenue"].map("${0:,.2f}".format)
purchasing_analysis["Average Price"] = purchasing_analysis["Average Price"].map("${0:,.2f}".format)

purchasing_analysis

```

    Purchasing Anlayis (Total)





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python

gender_purchase_data_pd = purchase_data_pd.groupby('Gender').agg({'SN':['nunique']})

gender_purchase_data_pd.reset_index(inplace=True)

gender_purchase_data_pd.columns = ["Gender", "Total count"] 

gender_purchase_data_pd["Percentage of Players"] = round(gender_purchase_data_pd["Total count"]/total_players_count*100,2)

gender_purchase_data_pd = gender_purchase_data_pd[["Gender","Percentage of Players", "Total count"]]

print("Gender Demographics")
gender_purchase_data_pd

```

    Gender Demographics





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Percentage of Players</th>
      <th>Total count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#purchase analysis by gender
purchase_count_by_gender = purchase_data_pd.groupby("Gender")["Price"].count()
purchase_count_by_gender 

total_purchase_value_by_Gender = purchase_data_pd.groupby("Gender")["Price"].sum()
total_purchase_value_by_Gender

avg_purchase_value_by_Gender = purchase_data_pd.groupby("Gender")["Price"].mean()
avg_purchase_value_by_Gender

gender_purchase_data_pd = purchase_data_pd.groupby('Gender').agg({'Price':['count','sum','mean']})

gender_purchase_data_pd.reset_index(inplace=True)

gender_purchase_data_pd.columns = ["Gender", "Purchase Count","Total Price", "Avg Price"] 

gender_purchase_data_pd["Total Price"] = gender_purchase_data_pd["Total Price"].map("${0:,.2f}".format)
gender_purchase_data_pd["Avg Price"] = gender_purchase_data_pd["Avg Price"].map("${0:,.2f}".format)

print("Purchase Analysis (Gender)")
gender_purchase_data_pd


```

    Purchase Analysis (Gender)





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Total Price</th>
      <th>Avg Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>136</td>
      <td>$382.91</td>
      <td>$2.82</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>633</td>
      <td>$1,867.68</td>
      <td>$2.95</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>11</td>
      <td>$35.74</td>
      <td>$3.25</td>
    </tr>
  </tbody>
</table>
</div>




```python
purchase_data_pd["Age"].min()
purchase_data_pd["Age"].max()

#7, 45
# Bins are 0 to 25, 25 to 50, 50 to 75, 75 to 100
bins = [0, 10,14,18,22,26,30,34,38,42,50]

# Create the names for the four bins
group_names = ['<=10', '11-14', '15-18', '19-22','23-26','27-30','31-34','35-38','39-42','>42']

# Cut postTestScore and place the scores into bins
pd.cut(purchase_data_pd["Age"], bins, labels=group_names)

purchase_data_pd["Age Bins"] = pd.cut(purchase_data_pd["Age"], bins, labels=group_names)


Age_bin_purchase_data_pd = purchase_data_pd.groupby('Age Bins').agg({'SN':['nunique']})

Age_bin_purchase_data_pd.reset_index(inplace=True)

Age_bin_purchase_data_pd.columns = ["Age Bins", "Total count"] 

Age_bin_purchase_data_pd["Percentage of Players"] = round(Age_bin_purchase_data_pd["Total count"]/total_players_count*100,2)

Age_bin_purchase_data_pd = Age_bin_purchase_data_pd[["Age Bins","Percentage of Players", "Total count"]]
print ("Age Demographics")
Age_bin_purchase_data_pd

```

    Age Demographics





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age Bins</th>
      <th>Percentage of Players</th>
      <th>Total count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;=10</td>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11-14</td>
      <td>3.49</td>
      <td>20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-18</td>
      <td>14.66</td>
      <td>84</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19-22</td>
      <td>31.06</td>
      <td>178</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23-26</td>
      <td>26.70</td>
      <td>153</td>
    </tr>
    <tr>
      <th>5</th>
      <td>27-30</td>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>6</th>
      <td>31-34</td>
      <td>5.93</td>
      <td>34</td>
    </tr>
    <tr>
      <th>7</th>
      <td>35-38</td>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>8</th>
      <td>39-42</td>
      <td>1.92</td>
      <td>11</td>
    </tr>
    <tr>
      <th>9</th>
      <td>&gt;42</td>
      <td>0.35</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#purchase analysis by age bins

Age_bins_purchase_data_pd = purchase_data_pd.groupby('Age Bins').agg({'Price':['count','sum','mean']})

Age_bins_purchase_data_pd.reset_index(inplace=True)

Age_bins_purchase_data_pd.columns = ["Age Bins", "Purchase Count","Total Price", "Avg Price"]

Age_bins_purchase_data_pd.sort_values(by='Total Price',ascending=False).head()

Age_bins_purchase_data_pd["Total Price"] = Age_bins_purchase_data_pd["Total Price"].map("${0:,.2f}".format)
Age_bins_purchase_data_pd["Avg Price"] = Age_bins_purchase_data_pd["Avg Price"].map("${0:,.2f}".format)

print("Purchase Analysis (Age)")
Age_bins_purchase_data_pd
```

    Purchase Analysis (Age)





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age Bins</th>
      <th>Purchase Count</th>
      <th>Total Price</th>
      <th>Avg Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;=10</td>
      <td>32</td>
      <td>$96.62</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11-14</td>
      <td>31</td>
      <td>$83.79</td>
      <td>$2.70</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-18</td>
      <td>111</td>
      <td>$319.32</td>
      <td>$2.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19-22</td>
      <td>231</td>
      <td>$676.20</td>
      <td>$2.93</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23-26</td>
      <td>207</td>
      <td>$608.02</td>
      <td>$2.94</td>
    </tr>
    <tr>
      <th>5</th>
      <td>27-30</td>
      <td>63</td>
      <td>$187.99</td>
      <td>$2.98</td>
    </tr>
    <tr>
      <th>6</th>
      <td>31-34</td>
      <td>46</td>
      <td>$141.24</td>
      <td>$3.07</td>
    </tr>
    <tr>
      <th>7</th>
      <td>35-38</td>
      <td>37</td>
      <td>$104.06</td>
      <td>$2.81</td>
    </tr>
    <tr>
      <th>8</th>
      <td>39-42</td>
      <td>20</td>
      <td>$62.56</td>
      <td>$3.13</td>
    </tr>
    <tr>
      <th>9</th>
      <td>&gt;42</td>
      <td>2</td>
      <td>$6.53</td>
      <td>$3.26</td>
    </tr>
  </tbody>
</table>
</div>




```python
#find top 5 spenders
players_purchase_data_pd = purchase_data_pd.groupby('SN').agg({'Price':['count','sum','mean']})

players_purchase_data_pd.reset_index(inplace=True)

players_purchase_data_pd.columns = ["SN", "Purchase Count","Total Price", "Avg Price"]

players_purchase_data_pd["Avg Price"] = players_purchase_data_pd["Avg Price"].map("${0:,.2f}".format)
players_purchase_data_pd["Total Price"] = players_purchase_data_pd["Total Price"].map("${0:,.2f}".format)

print("Top Spenders")
players_purchase_data_pd.sort_values(by='Total Price',ascending=False).head()


```

    Top Spenders





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Total Price</th>
      <th>Avg Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>385</th>
      <td>Qarwen67</td>
      <td>4</td>
      <td>$9.97</td>
      <td>$2.49</td>
    </tr>
    <tr>
      <th>471</th>
      <td>Sondim43</td>
      <td>3</td>
      <td>$9.38</td>
      <td>$3.13</td>
    </tr>
    <tr>
      <th>504</th>
      <td>Tillyrin30</td>
      <td>3</td>
      <td>$9.19</td>
      <td>$3.06</td>
    </tr>
    <tr>
      <th>327</th>
      <td>Lisistaya47</td>
      <td>3</td>
      <td>$9.19</td>
      <td>$3.06</td>
    </tr>
    <tr>
      <th>529</th>
      <td>Tyisriphos58</td>
      <td>2</td>
      <td>$9.18</td>
      <td>$4.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#find top 5 popular items by purchase count 
print("Most Popular Items")
item_purchase_data_pd = purchase_data_pd.groupby(['Item ID','Item Name']).agg({'Price':['count','sum','mean']})

item_purchase_data_pd.reset_index(inplace=True)

item_purchase_data_pd.columns = ["Item Id", "Item Name", "Purchase Count","Total Price", "Item Price"]

top5_by_purchase_count = item_purchase_data_pd.sort_values(by='Purchase Count',ascending=False).head()

top5_by_purchase_count
```

    Most Popular Items





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Id</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Price</th>
      <th>Item Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>25.85</td>
      <td>2.35</td>
    </tr>
    <tr>
      <th>84</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>24.53</td>
      <td>2.23</td>
    </tr>
    <tr>
      <th>31</th>
      <td>31</td>
      <td>Trickster</td>
      <td>9</td>
      <td>18.63</td>
      <td>2.07</td>
    </tr>
    <tr>
      <th>174</th>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>11.16</td>
      <td>1.24</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Serenity</td>
      <td>9</td>
      <td>13.41</td>
      <td>1.49</td>
    </tr>
  </tbody>
</table>
</div>




```python
#find top 5 popular items by purchase value
print("Most Profitable Items")
top5_by_purchase_value = item_purchase_data_pd.sort_values(by='Total Price',ascending=False).head()
top5_by_purchase_value

```

    Most Profitable Items





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Id</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Price</th>
      <th>Item Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>37.26</td>
      <td>4.14</td>
    </tr>
    <tr>
      <th>115</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>29.75</td>
      <td>4.25</td>
    </tr>
    <tr>
      <th>32</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>6</td>
      <td>29.70</td>
      <td>4.95</td>
    </tr>
    <tr>
      <th>103</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>6</td>
      <td>29.22</td>
      <td>4.87</td>
    </tr>
    <tr>
      <th>107</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>8</td>
      <td>28.88</td>
      <td>3.61</td>
    </tr>
  </tbody>
</table>
</div>


