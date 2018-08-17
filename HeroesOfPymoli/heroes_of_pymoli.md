
### Heroes Of Pymoli Data Analysis

* Males account for the lion share of players representing 84% of all players out of 576 players. However, females have a higher average purchase per person at \$4.47 versus the male value of $4.07.
* The largest proportion of players are ages 20-24 at 45% of the 576 players. However, players ages 35-39 purchase the most on average with an average spend of \$4.76.
* The most popular item by purchase count and total purchase value is "Oathbreaker, Last Hope of the Breaking Storm" with 12 purchases and a total value of \$50.76.


```python
# import dependencies
import pandas as pd
import numpy as np

# provide path to data file
csv_path = "Resources/purchase_data.csv"

# store game purchase data in a pandas dataframe
game_purchase_data = pd.read_csv(csv_path)
```

## Player Count
* Total Number of Players


```python
# count up all unique screen names in the data set and display in a data frame
unique_SN_count = len(game_purchase_data["SN"].unique())

player_count_df = pd.DataFrame({"Player Count": [unique_SN_count]}).set_index("Player Count")
player_count_df
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
    </tr>
    <tr>
      <th>Player Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>576</th>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)
 
* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue


```python
# first count the number of unique items
unique_item_count = len(game_purchase_data["Item ID"].unique())

# next get the average purchase price
average_price = '${:,.2f}'.format(game_purchase_data["Price"].mean())

# next get total number of purchases
purchase_count = game_purchase_data["Purchase ID"].count()

# next get total revenue
total_revenue = '${:,.2f}'.format(game_purchase_data["Price"].sum())

# store and display all results in a pandas dataframe
purchase_analysis_df = pd.DataFrame({
    "Average Price": [average_price],
    "Number of Purchases": [purchase_count],
    "Number of Unique Items": [unique_item_count],
    "Total Revenue": [total_revenue]
}).set_index("Average Price")
purchase_analysis_df
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
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
    <tr>
      <th>Average Price</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>$3.05</th>
      <td>780</td>
      <td>183</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed


```python
# get types of gender entries
gender_entries = game_purchase_data["Gender"].unique()
# types of gender entry are: 'Male', 'Other / Non-Disclosed', 'Female'

# get counts for each gender entry
male_count = len(game_purchase_data.loc[game_purchase_data["Gender"] == "Male", :]["SN"].unique())
female_count = len(game_purchase_data.loc[game_purchase_data["Gender"] == "Female", :]["SN"].unique())
other_count = len(game_purchase_data.loc[game_purchase_data["Gender"] == "Other / Non-Disclosed", :]["SN"].unique())

# calculate proportions for each gender entry of total players
male_proportion = '{:,.2f}%'.format((male_count / (male_count + female_count + other_count))*100)
female_proportion = '{:,.2f}%'.format((female_count / (male_count + female_count + other_count))*100)
other_proportion = '{:,.2f}%'.format((other_count / (male_count + female_count + other_count))*100)

# store results in a pandas dataframe and display
gender_df = pd.DataFrame({
    "Total Count": [male_count, female_count, other_count],
    "Percentage of Players": [male_proportion, female_proportion, other_proportion]
}).rename({0: "Male", 1: "Female", 2: "Other / Non-Disclosed"})
gender_df
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>484</td>
      <td>84.03%</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>81</td>
      <td>14.06%</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>1.91%</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)

* The below each broken by gender
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Gender


```python
# get count of purchases for each gender
male_purchase_count = game_purchase_data.loc[game_purchase_data["Gender"] == "Male", :]["Purchase ID"].count()
female_purchase_count = game_purchase_data.loc[game_purchase_data["Gender"] == "Female", :]["Purchase ID"].count()
other_purchase_count = game_purchase_data.loc[game_purchase_data["Gender"] == "Other / Non-Disclosed", :]["Purchase ID"].count()

# get average purchase price for each gender
male_average_price = '${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Gender"] == "Male", :]["Price"].mean())
female_average_price = '${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Gender"] == "Female", :]["Price"].mean())
other_average_price = '${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Gender"] == "Other / Non-Disclosed", :]["Price"].mean())

# get total purchase value for each gender
male_purchase_value = '${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Gender"] == "Male", :]["Price"].sum())
female_purchase_value = '${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Gender"] == "Female", :]["Price"].sum())
other_purchase_value = '${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Gender"] == "Other / Non-Disclosed", :]["Price"].sum())

# get average purchase total per person by gender
male_per_person = '${:,.2f}'.format((game_purchase_data.loc[game_purchase_data["Gender"] == "Male", :]["Price"].sum()) / male_count)
female_per_person = '${:,.2f}'.format((game_purchase_data.loc[game_purchase_data["Gender"] == "Female", :]["Price"].sum()) / female_count)
other_per_person = '${:,.2f}'.format((game_purchase_data.loc[game_purchase_data["Gender"] == "Other / Non-Disclosed", :]["Price"].sum()) / other_count)

# place all values in data frame and display
gender_purchase_df = pd.DataFrame({
    "Purchase Count": [male_purchase_count, female_purchase_count, other_purchase_count],
    "Average Purchase Price": [male_average_price, female_average_price, other_average_price],
    "Total Purchase Value": [male_purchase_value, female_purchase_value, other_purchase_value],
    "Avg Total Purchase per Person": [male_per_person, female_per_person, other_per_person]
}).rename({0: "Male", 1: "Female", 2: "Other / Non-Disclosed"})
gender_purchase_df
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
      <th>Avg Total Purchase per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



### Age Demographics

* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Player Counts and Proportions
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age Group

### Purchase Count by Age Group


```python
# create age bins and store into dataframe of dataset
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
age_bin_names = ["<10", "10-14", "15-19", "20-24", "25-29", 
                 "30-34", "35-39", "40+"]
game_purchase_data["Age Group"] = pd.cut(game_purchase_data["Age"], age_bins, 
                                         labels=age_bin_names)

# get the count of purchases within each age group and store in a list
age_count = []
for age in age_bin_names:
    age_count.append(len(game_purchase_data.loc[game_purchase_data["Age Group"] == age, :]["SN"].unique()))

# calculate proportion of players that belong to each age group and store in a list
age_proportion = []
for count in age_count:
    age_proportion.append('{:,.2f}%'.format((count / unique_SN_count) * 100))

# store results in a dataframe and display it
age_purchase_df = pd.DataFrame({
    "Age Group": age_bin_names,
    "Total Count": age_count,
    "Percentage of Players": age_proportion
}).set_index("Age Group")
age_purchase_df
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
  </tbody>
</table>
</div>



###  Purchasing Analysis (Age)


```python
# store the count of purchases for each age group in a list
age_purchase_count = []
for age in age_bin_names:
    age_purchase_count.append(game_purchase_data.loc[game_purchase_data["Age Group"] == age, :]["Purchase ID"].count())

# store the average purchase price for each age group in a list
age_average_price = []
for age in age_bin_names:
    age_average_price.append('${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Age Group"] == age, :]["Price"].mean()))

# store the total purchase value for each age group in a list
age_purchase_value = []
age_purchase_value_hold = []
for age in age_bin_names:
    age_purchase_value.append('${:,.2f}'.format(game_purchase_data.loc[game_purchase_data["Age Group"] == age, :]["Price"].sum()))
    age_purchase_value_hold.append(game_purchase_data.loc[game_purchase_data["Age Group"] == age, :]["Price"].sum())
    

# store the average total purchase per person per age group in a list
age_avg_per_person = ['${:,.2f}'.format(element) for element in (np.array(age_purchase_value_hold) / np.array(age_count))]

# store results in a dataframe and display it
age_purchase_analysis_df = pd.DataFrame({
    "Age Group": age_bin_names,
    "Purchase Count": age_purchase_count,
    "Average Purchase Price": age_average_price,
    "Total Purchase Value": age_purchase_value,
    "Avg Total Purchase per Person": age_avg_per_person
}).set_index("Age Group")
age_purchase_analysis_df
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
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



### Top Spenders

* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
    * SN
    * Purchase Count
    * Average Purchase Price
    * Total Purchase Value


```python
# first get list of unique users
unique_users_list = game_purchase_data["SN"].unique()
users_purchase_count = []
users_total_purchase_value = []

# loop through users and sum up the count of their purchases and their total purchase value
for user in unique_users_list:
    users_purchase_count.append(game_purchase_data.loc[game_purchase_data["SN"] == user, :]["SN"].count())
    users_total_purchase_value.append(game_purchase_data.loc[game_purchase_data["SN"] == user, :]["Price"].sum())

# calculate the average purchase price per user within another list
users_average_price = ['${:,.2f}'.format(element) for element in (np.array(users_total_purchase_value) / np.array(users_purchase_count))]

# place all of the lists into a data frame
spenders_df = pd.DataFrame({
    "Screen Name": unique_users_list,
    "Count of Purchases": users_purchase_count,
    "Average Purchase Price": users_average_price,
    "Total Purchase Value": users_total_purchase_value
}).set_index("Screen Name")

# sort dataframe by total purchase value
sorted_spenders_df = spenders_df.sort_values("Total Purchase Value", ascending=False)

# format the purchase value as dollar currency format
sorted_spenders_df["Total Purchase Value"] = sorted_spenders_df["Total Purchase Value"].astype(float).map("${:,.2f}".format)

# display the top 5 users by purchase value
sorted_spenders_df.head()
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
      <th>Count of Purchases</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Screen Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



### Most Popular Items

* Identify the 5 most popular items by purchase count, then list (in a table):
    * Item ID
    * Item Name
    * Purchase Count
    * Item Price
    * Total Purchase Value


```python
# first get list of unique item IDs
unique_item_ids = game_purchase_data["Item ID"].unique()

# get list of item names, purchase counts, item price and total purchase value
# associated with unique item id and store in lists
item_names = []
item_purchase_counts = []
item_price = []
item_purchase_value = []
for item_id in unique_item_ids:
    item_names.append(game_purchase_data.loc[game_purchase_data["Item ID"] == item_id, :]["Item Name"].iloc[0])
    item_purchase_counts.append(game_purchase_data.loc[game_purchase_data["Item ID"] == item_id, :]["Purchase ID"].count())
    item_price.append(game_purchase_data.loc[game_purchase_data["Item ID"] == item_id, :]["Price"].iloc[0])
    item_purchase_value.append(game_purchase_data.loc[game_purchase_data["Item ID"] == item_id, :]["Price"].sum())

# create dataframe for items
items_df = pd.DataFrame({
    "Item ID": unique_item_ids,
    "Item Name": item_names,
    "Item Purchase Count": item_purchase_counts,
    "Item Price": item_price,
    "Item Total Purchase Value": item_purchase_value
}).set_index("Item ID")

# sort item dataframe by item purchase count
sorted_items_df = items_df.sort_values("Item Purchase Count", ascending=False)

# format the item price and purchase value columns as dollar currency format
sorted_items_df["Item Price"] = sorted_items_df["Item Price"].astype(float).map("${:,.2f}".format)
sorted_items_df["Item Total Purchase Value"] = sorted_items_df["Item Total Purchase Value"].astype(float).map("${:,.2f}".format)

# display the top 5 users by purchase value
sorted_items_df.head()
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
      <th>Item Name</th>
      <th>Item Purchase Count</th>
      <th>Item Price</th>
      <th>Item Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Nirvana</td>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Pursuit, Cudgel of Necromancy</td>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>



### Most Profitable Items

* Identify the 5 most profitable items by total purchase value, then list (in a table):
    * Item ID
    * Item Name
    * Purchase Count
    * Item Price
    * Total Purchase Value


```python
# simply resort the previous dataframe along total purchase value
purchase_sorted_items_df = items_df.sort_values("Item Total Purchase Value", ascending=False)

# format the item price and purchase value columns as dollar currency format
purchase_sorted_items_df["Item Price"] = purchase_sorted_items_df["Item Price"].astype(float).map("${:,.2f}".format)
purchase_sorted_items_df["Item Total Purchase Value"] = purchase_sorted_items_df["Item Total Purchase Value"].astype(float).map("${:,.2f}".format)

# display the top 5 users by purchase value
purchase_sorted_items_df.head()
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
      <th>Item Name</th>
      <th>Item Purchase Count</th>
      <th>Item Price</th>
      <th>Item Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Nirvana</td>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Final Critic</td>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


