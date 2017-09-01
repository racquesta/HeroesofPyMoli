
# Heroes of Pymoli Data Analysis

+ Only 1 of the 6 most popular items appear in the list of the 18 lowest priced items/items prices in the bottom 10% of item prices.  (See Popular Items and Other Analysis - Lowest Priced)
+ Males not only the make up over 80% of the players of this game, the are also responsible for over 80% of the revenue.  (See Gender Demographics and Other Analysis - Gender Purcahse Totals)
+ Out of the 573 players, each player has spend under 20 dollars on items.  
+ The 20-24 age bracket spends has generated more revenue than any other bracket, but the 25-29 age group, on average, purchases more expensive items.  
+ The Retribution Axe not only appears in the most popular item list, it is priced almost 2 dollars more than other popular items on that list and is the item that has generated the most revenue. 
+ Other than the Retribution Axe, the most popular items list generates items that are priced below the average purcahse price.  



```python
#Dependencies and file read
import pandas as pd
import numpy as np
import os

file = os.path.join('Resources', 'purchase_data.json')

pur_data = pd.read_json(file)

#view the data
#pur_data.head()
```

## Player Count


```python
#Find player count by finding unique screen names and finding the length of that list
player_count = len(pur_data['SN'].unique())

# DataFrame creation for player count
players_df = pd.DataFrame([{'Total Players': player_count}])
#gets rid of number index and resets to Total Players 
players_df.set_index('Total Players', inplace = True)
players_df
```




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
    </tr>
    <tr>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>573</th>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
#code for inspecting data
#pur_data['Item ID'].value_counts()
#unique_items = pd.DataFrame(pur_data['Item ID'].unique())
#len(unique_items)

#creates a df but only keeping last occurance of Item ID
no_dup_items = pur_data.drop_duplicates(['Item ID'], keep = 'last')
#counts items by unique ID
total_unique = len(no_dup_items)
#finds the number of total purchases by counting occurances of price
total_pur = pur_data['Price'].count()
#calculates total revenue for table by summing occurance of price and below calc
total_rev = round(pur_data['Price'].sum(),2)
#calculates total_rev
avg_price = round(total_rev/total_pur, 2)

#creates Purchase Analysis DataFrame
pur_analysis = pd.DataFrame([{
    
    "Number of Unique Items": total_unique,
    'Average Purchase Price': avg_price,
    'Total Purchases': total_pur,
    'Total Revenue': total_rev
}])

#format Purchases Analysis Table
pur_analysis.style.format({'Average Purchase Price': '${:.2f}', 'Total Revenue': '${:,.2f}'})

```




<style  type="text/css" >
</style>  
<table id="T_1a81f558_8f6b_11e7_85b4_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Number of Unique Items</th> 
        <th class="col_heading level0 col2" >Total Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1a81f558_8f6b_11e7_85b4_d4619d1535aa" class="row_heading level0 row0" >0</th> 
        <td id="T_1a81f558_8f6b_11e7_85b4_d4619d1535aarow0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_1a81f558_8f6b_11e7_85b4_d4619d1535aarow0_col1" class="data row0 col1" >183</td> 
        <td id="T_1a81f558_8f6b_11e7_85b4_d4619d1535aarow0_col2" class="data row0 col2" >780</td> 
        <td id="T_1a81f558_8f6b_11e7_85b4_d4619d1535aarow0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 



## Gender Demographics


```python
# Gender Demographics

# Percentage and Count of Male Players
# Percentage and Count of Female Players
# Percentage and Count of Other / Non-Disclosed

#creates df of unique player names by only keeping the last occurance
no_dup_players = pur_data.drop_duplicates(['SN'], keep ='last')

#counts gender values from the df with no duplicate screen names
gender_counts = no_dup_players['Gender'].value_counts().reset_index()
#adds column for % of players using player count from first table and gender_count 
#column which is a count from line above
gender_counts['% of Players'] = gender_counts['Gender']/player_count * 100
#renames columns
gender_counts.rename(columns = {'index': 'Gender', 'Gender': '# of Players'}, inplace = True)
#sets index as Gender for aesthetics 
gender_counts.set_index(['Gender'], inplace = True)
#just checking percents sum to 100%
#gender_counts['% of Players'].sum()
#formats table
gender_counts.style.format({"% of Players": "{:.1f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Players</th> 
        <th class="col_heading level0 col1" >% of Players</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aa" class="row_heading level0 row0" >Male</th> 
        <td id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aarow0_col0" class="data row0 col0" >465</td> 
        <td id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aarow0_col1" class="data row0 col1" >81.2%</td> 
    </tr>    <tr> 
        <th id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aa" class="row_heading level0 row1" >Female</th> 
        <td id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aarow1_col0" class="data row1 col0" >100</td> 
        <td id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aarow1_col1" class="data row1 col1" >17.5%</td> 
    </tr>    <tr> 
        <th id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aa" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aarow2_col0" class="data row2 col0" >8</td> 
        <td id="T_99fd8a8c_8f68_11e7_ad65_d4619d1535aarow2_col1" class="data row2 col1" >1.4%</td> 
    </tr></tbody> 
</table> 



## Purchasing Analysis by Gender


```python
# Purchasing Analysis (Gender)

# The below each broken by gender
# Purchase Count
# Average Purchase Price
# Total Purchase Value
# Normalized Totals

# counts purchases by gender
pur_count_by_gen = pd.DataFrame(pur_data.groupby('Gender')['Gender'].count())
# sums price by gender
total_pur_by_gen = pd.DataFrame(pur_data.groupby('Gender')['Price'].sum())
#merges the two data frames from above
pur_analysis_gen = pd.merge(pur_count_by_gen, total_pur_by_gen, left_index = True, right_index = True)
#renames columns
pur_analysis_gen.rename(columns = {'Gender': '# of Purchases', 'Price':'Total Purchase Value'}, inplace=True)
#adds column for average purchase price by gender by dividing total purcahse value by gender by # of purchases by gender
pur_analysis_gen['Average Purchase Price'] = pur_analysis_gen['Total Purchase Value']/pur_analysis_gen['# of Purchases']
#merges gender counts from above table (excluding dup SNs) into current df 
pur_analysis_gen = pur_analysis_gen.merge(gender_counts, left_index = True, right_index = True)
# calculates and adds normalized total column by dividing total purchase value by unique # of players by genger
pur_analysis_gen['Normalized Totals'] = pur_analysis_gen['Total Purchase Value']/pur_analysis_gen['# of Players']
pur_analysis_gen
#deletes columns not needed for table (# of Players was used for normalized totals while % of players came from gender count table)
del pur_analysis_gen['% of Players']
del pur_analysis_gen['# of Players']
# #resets index for aesthetics 
# # pur_analysis_gen.set_index('Gender', inplace=True)
# #formats table
pur_analysis_gen.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}', 'Normalized Totals': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Purchases</th> 
        <th class="col_heading level0 col1" >Total Purchase Value</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aa" class="row_heading level0 row0" >Female</th> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow0_col0" class="data row0 col0" >136</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow0_col1" class="data row0 col1" >$382.91</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow0_col2" class="data row0 col2" >$2.82</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow0_col3" class="data row0 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aa" class="row_heading level0 row1" >Male</th> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow1_col0" class="data row1 col0" >633</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow1_col1" class="data row1 col1" >$1867.68</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow1_col2" class="data row1 col2" >$2.95</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow1_col3" class="data row1 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aa" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow2_col0" class="data row2 col0" >11</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow2_col1" class="data row2 col1" >$35.74</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow2_col2" class="data row2 col2" >$3.25</td> 
        <td id="T_fef2f2a6_8f6a_11e7_8b4e_d4619d1535aarow2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 



## Age Demographics


```python
# The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
# Purchase Count
# Average Purchase Price
# Total Purchase Value
# Normalized Totals

#creates a column 'age_bin' based on conditional of age range
pur_data.loc[(pur_data['Age'] < 10), 'age_bin'] = "< 10"
pur_data.loc[(pur_data['Age'] >= 10) & (pur_data['Age'] <= 14), 'age_bin'] = "10 - 14"
pur_data.loc[(pur_data['Age'] >= 15) & (pur_data['Age'] <= 19), 'age_bin'] = "15 - 19"
pur_data.loc[(pur_data['Age'] >= 20) & (pur_data['Age'] <= 24), 'age_bin'] = "20 - 24"
pur_data.loc[(pur_data['Age'] >= 25) & (pur_data['Age'] <= 29), 'age_bin'] = "25 - 29"
pur_data.loc[(pur_data['Age'] >= 30) & (pur_data['Age'] <= 34), 'age_bin'] = "30 - 34"
pur_data.loc[(pur_data['Age'] >= 35) & (pur_data['Age'] <= 39), 'age_bin'] = "35 - 39"
pur_data.loc[(pur_data['Age'] >= 40), 'age_bin'] = "> 40"
#double checked count
# pur_data[['age_bin', 'Age']].count()

# counts purchases by age bin by counting screen names (non-unique)
pur_count_age = pd.DataFrame(pur_data.groupby('age_bin')['SN'].count())
#finds avg price of purchases by age bin
avg_price_age = pd.DataFrame(pur_data.groupby('age_bin')['Price'].mean())
#finds total purchase value by age bin
tot_pur_age = pd.DataFrame(pur_data.groupby('age_bin')['Price'].sum())
#deletes multiple occurances of SN while only keeping last, then counts # of unique
#players by age bin
no_dup_age = pd.DataFrame(pur_data.drop_duplicates('SN', keep = 'last').groupby('age_bin')['SN'].count())
#merges all info from above into one df
merge_age = pd.merge(pur_count_age, avg_price_age, left_index = True, right_index = True).merge(tot_pur_age, left_index = True, right_index = True).merge(no_dup_age, left_index = True, right_index = True)
#renames columns
merge_age.rename(columns = {"SN_x": "# of Purchases", "Price_x": "Average Purchase Price", "Price_y": "Total Purchase Value", "SN_y": "# of Purchasers"}, inplace = True)
#calculates normalized totals
merge_age['Normalized Totals'] = merge_age['Total Purchase Value']/merge_age['# of Purchasers']
#rest index for aesthetics
merge_age.index.rename("Age", inplace = True)
# formats
merge_age.style.format({'Average Purchase Price': '${:.2f}', 'Total Purchase Value': '${:.2f}', 'Normalized Totals': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Purchases</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" ># of Purchasers</th> 
        <th class="col_heading level0 col4" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row0" >10 - 14</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow0_col0" class="data row0 col0" >35</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow0_col1" class="data row0 col1" >$2.77</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow0_col2" class="data row0 col2" >$96.95</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow0_col3" class="data row0 col3" >23</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow0_col4" class="data row0 col4" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row1" >15 - 19</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow1_col0" class="data row1 col0" >133</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow1_col1" class="data row1 col1" >$2.91</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow1_col2" class="data row1 col2" >$386.42</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow1_col3" class="data row1 col3" >100</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow1_col4" class="data row1 col4" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row2" >20 - 24</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow2_col0" class="data row2 col0" >336</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow2_col2" class="data row2 col2" >$978.77</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow2_col3" class="data row2 col3" >259</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow2_col4" class="data row2 col4" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row3" >25 - 29</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow3_col0" class="data row3 col0" >125</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow3_col1" class="data row3 col1" >$2.96</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow3_col2" class="data row3 col2" >$370.33</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow3_col3" class="data row3 col3" >87</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow3_col4" class="data row3 col4" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row4" >30 - 34</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow4_col0" class="data row4 col0" >64</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow4_col1" class="data row4 col1" >$3.08</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow4_col2" class="data row4 col2" >$197.25</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow4_col3" class="data row4 col3" >47</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow4_col4" class="data row4 col4" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row5" >35 - 39</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow5_col0" class="data row5 col0" >42</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow5_col1" class="data row5 col1" >$2.84</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow5_col2" class="data row5 col2" >$119.40</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow5_col3" class="data row5 col3" >27</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow5_col4" class="data row5 col4" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row6" >< 10</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow6_col0" class="data row6 col0" >28</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow6_col1" class="data row6 col1" >$2.98</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow6_col2" class="data row6 col2" >$83.46</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow6_col3" class="data row6 col3" >19</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow6_col4" class="data row6 col4" >$4.39</td> 
    </tr>    <tr> 
        <th id="T_9a13734c_8f68_11e7_beb4_d4619d1535aa" class="row_heading level0 row7" >> 40</th> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow7_col0" class="data row7 col0" >17</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow7_col1" class="data row7 col1" >$3.16</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow7_col2" class="data row7 col2" >$53.75</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow7_col3" class="data row7 col3" >11</td> 
        <td id="T_9a13734c_8f68_11e7_beb4_d4619d1535aarow7_col4" class="data row7 col4" >$4.89</td> 
    </tr></tbody> 
</table> 



# Top Spenders


```python
# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
# SN
# Purchase Count
# Average Purchase Price
# Total Purchase Value

#Group by screen name to find, total purchase per person, number of purchases per person, and average price price per person
purchase_amt_by_SN = pd.DataFrame(pur_data.groupby('SN')['Price'].sum())
num_purchase_by_SN = pd.DataFrame(pur_data.groupby('SN')['Price'].count())
avg_purchase_by_SN = pd.DataFrame(pur_data.groupby('SN')['Price'].mean())
# merge the above dfs
merged_top5 = pd.merge(purchase_amt_by_SN, num_purchase_by_SN, left_index = True, right_index = True).merge(avg_purchase_by_SN, left_index=True, right_index=True)
# rename columns
merged_top5.rename(columns = {'Price_x': 'Total Purchase Value', 'Price_y':'Purchase Count', 'Price':'Average Purchase Price'}, inplace = True)
# sort from highest purchase value to lowest
merged_top5.sort_values('Total Purchase Value', ascending = False, inplace=True)
# take top 5 only
merged_top5 = merged_top5.head()
# format
merged_top5.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Purchase Value</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aa" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow0_col0" class="data row0 col0" >$17.06</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow0_col1" class="data row0 col1" >5</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow0_col2" class="data row0 col2" >$3.41</td> 
    </tr>    <tr> 
        <th id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aa" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow1_col0" class="data row1 col0" >$13.56</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow1_col1" class="data row1 col1" >4</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow1_col2" class="data row1 col2" >$3.39</td> 
    </tr>    <tr> 
        <th id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aa" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow2_col0" class="data row2 col0" >$12.74</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow2_col1" class="data row2 col1" >4</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow2_col2" class="data row2 col2" >$3.18</td> 
    </tr>    <tr> 
        <th id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aa" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow3_col0" class="data row3 col0" >$12.73</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow3_col1" class="data row3 col1" >3</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow3_col2" class="data row3 col2" >$4.24</td> 
    </tr>    <tr> 
        <th id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aa" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow4_col0" class="data row4 col0" >$11.58</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow4_col1" class="data row4 col1" >3</td> 
        <td id="T_9a18d1c2_8f68_11e7_9fd6_d4619d1535aarow4_col2" class="data row4 col2" >$3.86</td> 
    </tr></tbody> 
</table> 



## Most Popular Items


```python
# Identify the 5 most popular items by purchase count, then list (in a table):
# Item ID
# Item Name
# Purchase Count
# Item Price
# Total Purchase Value

# gets a count of each item by grouping by Item ID and counting the number of each IDs occurances
top5_items_ID = pd.DataFrame(pur_data.groupby('Item ID')['Item ID'].count())
#sort from high to low total purchase count
top5_items_ID.sort_values('Item ID', ascending = False, inplace = True)
#keep the first 6 rows because there is a tie
top5_items_ID = top5_items_ID.iloc[0:6][:]
#find the total purchase value of each item
top5_items_total = pd.DataFrame(pur_data.groupby('Item ID')['Price'].sum())
#merge purcahse count and total purcahse value 
top5_items = pd.merge(top5_items_ID, top5_items_total, left_index = True, right_index = True)
#drop duplicate items from original Df
no_dup_items = pur_data.drop_duplicates(['Item ID'], keep = 'last')
# merge to get all other info from the top 6 using the no dup df
top5_merge_ID = pd.merge(top5_items, no_dup_items, left_index = True, right_on = 'Item ID')
#keep only neede columns
top5_merge_ID = top5_merge_ID[['Item ID', 'Item Name', 'Item ID_x', 'Price_y', 'Price_x']]
#reset index as item ID for aesthetics
top5_merge_ID.set_index(['Item ID'], inplace = True)
# rename columns
top5_merge_ID.rename(columns =  {'Item ID_x': 'Purchase Count', 'Price_y': 'Item Price', 'Price_x': 'Total Purchase Value'}, inplace=True)
#format
top5_merge_ID.style.format({'Item Price': '${:.2f}', 'Total Purchase Value': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Item Price</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" class="row_heading level0 row0" >39</th> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow0_col0" class="data row0 col0" >Betrayal, Whisper of Grieving Widows</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow0_col1" class="data row0 col1" >11</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow0_col2" class="data row0 col2" >$2.35</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow0_col3" class="data row0 col3" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" class="row_heading level0 row1" >84</th> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow1_col0" class="data row1 col0" >Arcane Gem</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow1_col1" class="data row1 col1" >11</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow1_col2" class="data row1 col2" >$2.23</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow1_col3" class="data row1 col3" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" class="row_heading level0 row2" >31</th> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow2_col0" class="data row2 col0" >Trickster</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow2_col1" class="data row2 col1" >9</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow2_col2" class="data row2 col2" >$2.07</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow2_col3" class="data row2 col3" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" class="row_heading level0 row3" >175</th> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow3_col0" class="data row3 col0" >Woeful Adamantite Claymore</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow3_col1" class="data row3 col1" >9</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow3_col2" class="data row3 col2" >$1.24</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow3_col3" class="data row3 col3" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" class="row_heading level0 row4" >13</th> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow4_col0" class="data row4 col0" >Serenity</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow4_col1" class="data row4 col1" >9</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow4_col2" class="data row4 col2" >$1.49</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow4_col3" class="data row4 col3" >$13.41</td> 
    </tr>    <tr> 
        <th id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aa" class="row_heading level0 row5" >34</th> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow5_col0" class="data row5 col0" >Retribution Axe</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow5_col1" class="data row5 col1" >9</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow5_col2" class="data row5 col2" >$4.14</td> 
        <td id="T_9a24e20a_8f68_11e7_bf4a_d4619d1535aarow5_col3" class="data row5 col3" >$37.26</td> 
    </tr></tbody> 
</table> 



## Most Profitable Items


```python
# Most Profitable Items

# Identify the 5 most profitable items by total purchase value, then list (in a table):
# Item ID
# Item Name
# Purchase Count
# Item Price
# Total Purchase Value

# find total purcahse value and sort by high to low
top5_profit = pd.DataFrame(pur_data.groupby('Item ID')['Price'].sum())
top5_profit.sort_values('Price', ascending = False, inplace = True)
# only keep top 5
top5_profit = top5_profit.iloc[0:5][:]
#get item purchase count
pur_count_profit = pd.DataFrame(pur_data.groupby('Item ID')['Item ID'].count())

top5_profit = pd.merge(top5_profit, pur_count_profit, left_index = True, right_index = True, how = 'left')
top5_merge_profit = pd.merge(top5_profit, no_dup_items, left_index = True, right_on = 'Item ID', how = 'left')
top5_merge_profit = top5_merge_profit[['Item ID', 'Item Name', 'Item ID_x', 'Price_y','Price_x']]
top5_merge_profit.set_index(['Item ID'], inplace=True)
top5_merge_profit.rename(columns = {'Item ID_x': 'Purchase Count', 'Price_y': 'Item Price', 'Price_x': 'Total Purchase Value'}, inplace = True)
top5_merge_profit.style.format({'Item Price': '${:.2f}', 'Total Purchase Value': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_9a306454_8f68_11e7_8c39_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Item Price</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9a306454_8f68_11e7_8c39_d4619d1535aa" class="row_heading level0 row0" >34</th> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow0_col0" class="data row0 col0" >Retribution Axe</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow0_col1" class="data row0 col1" >9</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow0_col2" class="data row0 col2" >$4.14</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow0_col3" class="data row0 col3" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_9a306454_8f68_11e7_8c39_d4619d1535aa" class="row_heading level0 row1" >115</th> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow1_col0" class="data row1 col0" >Spectral Diamond Doomblade</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow1_col1" class="data row1 col1" >7</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow1_col2" class="data row1 col2" >$4.25</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow1_col3" class="data row1 col3" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_9a306454_8f68_11e7_8c39_d4619d1535aa" class="row_heading level0 row2" >32</th> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow2_col0" class="data row2 col0" >Orenmir</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow2_col1" class="data row2 col1" >6</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow2_col2" class="data row2 col2" >$4.95</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow2_col3" class="data row2 col3" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_9a306454_8f68_11e7_8c39_d4619d1535aa" class="row_heading level0 row3" >103</th> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow3_col0" class="data row3 col0" >Singed Scalpel</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow3_col1" class="data row3 col1" >6</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow3_col2" class="data row3 col2" >$4.87</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow3_col3" class="data row3 col3" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_9a306454_8f68_11e7_8c39_d4619d1535aa" class="row_heading level0 row4" >107</th> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow4_col0" class="data row4 col0" >Splitter, Foe Of Subtlety</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow4_col1" class="data row4 col1" >8</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow4_col2" class="data row4 col2" >$3.61</td> 
        <td id="T_9a306454_8f68_11e7_8c39_d4619d1535aarow4_col3" class="data row4 col3" >$28.88</td> 
    </tr></tbody> 
</table> 



## Other Analysis - Highest Priced Items


```python
highest_priced = no_dup_items.sort_values('Price', ascending = False)
highest_priced[['Item ID', 'Item Name', 'Price']].head(18)
```




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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>657</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>4.95</td>
    </tr>
    <tr>
      <th>670</th>
      <td>177</td>
      <td>Winterthorn, Defender of Shifting Worlds</td>
      <td>4.89</td>
    </tr>
    <tr>
      <th>716</th>
      <td>103</td>
      <td>Singed Scalpel</td>
      <td>4.87</td>
    </tr>
    <tr>
      <th>336</th>
      <td>173</td>
      <td>Stormfury Longsword</td>
      <td>4.83</td>
    </tr>
    <tr>
      <th>419</th>
      <td>42</td>
      <td>The Decapitator</td>
      <td>4.82</td>
    </tr>
    <tr>
      <th>436</th>
      <td>131</td>
      <td>Fury</td>
      <td>4.82</td>
    </tr>
    <tr>
      <th>398</th>
      <td>96</td>
      <td>Blood-Forged Skeletal Spine</td>
      <td>4.77</td>
    </tr>
    <tr>
      <th>455</th>
      <td>137</td>
      <td>Aetherius, Boon of the Blessed</td>
      <td>4.75</td>
    </tr>
    <tr>
      <th>686</th>
      <td>46</td>
      <td>Hopeless Ebon Dualblade</td>
      <td>4.75</td>
    </tr>
    <tr>
      <th>743</th>
      <td>134</td>
      <td>Undead Crusader</td>
      <td>4.67</td>
    </tr>
    <tr>
      <th>549</th>
      <td>135</td>
      <td>Warped Diamond Crusader</td>
      <td>4.66</td>
    </tr>
    <tr>
      <th>737</th>
      <td>101</td>
      <td>Final Critic</td>
      <td>4.62</td>
    </tr>
    <tr>
      <th>613</th>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
    </tr>
    <tr>
      <th>567</th>
      <td>181</td>
      <td>Reaper's Toll</td>
      <td>4.56</td>
    </tr>
    <tr>
      <th>421</th>
      <td>150</td>
      <td>Deathraze</td>
      <td>4.54</td>
    </tr>
    <tr>
      <th>300</th>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
    </tr>
    <tr>
      <th>411</th>
      <td>7</td>
      <td>Thorn, Satchel of Dark Souls</td>
      <td>4.51</td>
    </tr>
    <tr>
      <th>741</th>
      <td>145</td>
      <td>Fiery Glass Crusader</td>
      <td>4.45</td>
    </tr>
  </tbody>
</table>
</div>



## Other Analysis - Lowest Priced


```python
lowest_priced = no_dup_items.sort_values('Price', ascending = True)
lowest_priced[['Item ID', 'Item Name', 'Price']].head(18)
```




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
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>667</th>
      <td>15</td>
      <td>Soul Infused Crystal</td>
      <td>1.03</td>
    </tr>
    <tr>
      <th>771</th>
      <td>25</td>
      <td>Hero Cane</td>
      <td>1.03</td>
    </tr>
    <tr>
      <th>624</th>
      <td>95</td>
      <td>Singed Onyx Warscythe</td>
      <td>1.03</td>
    </tr>
    <tr>
      <th>723</th>
      <td>69</td>
      <td>Frenzy, Defender of the Harvest</td>
      <td>1.06</td>
    </tr>
    <tr>
      <th>430</th>
      <td>74</td>
      <td>Yearning Crusher</td>
      <td>1.06</td>
    </tr>
    <tr>
      <th>720</th>
      <td>82</td>
      <td>Nirvana</td>
      <td>1.11</td>
    </tr>
    <tr>
      <th>774</th>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>1.14</td>
    </tr>
    <tr>
      <th>647</th>
      <td>156</td>
      <td>Soul-Forged Steel Shortsword</td>
      <td>1.16</td>
    </tr>
    <tr>
      <th>467</th>
      <td>41</td>
      <td>Orbit</td>
      <td>1.16</td>
    </tr>
    <tr>
      <th>756</th>
      <td>6</td>
      <td>Rusty Skull</td>
      <td>1.20</td>
    </tr>
    <tr>
      <th>767</th>
      <td>122</td>
      <td>Unending Tyranny</td>
      <td>1.21</td>
    </tr>
    <tr>
      <th>761</th>
      <td>175</td>
      <td>Woeful Adamantite Claymore</td>
      <td>1.24</td>
    </tr>
    <tr>
      <th>656</th>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
    </tr>
    <tr>
      <th>750</th>
      <td>86</td>
      <td>Stormfury Lantern</td>
      <td>1.28</td>
    </tr>
    <tr>
      <th>712</th>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>1.32</td>
    </tr>
    <tr>
      <th>689</th>
      <td>33</td>
      <td>Curved Axe</td>
      <td>1.35</td>
    </tr>
    <tr>
      <th>776</th>
      <td>104</td>
      <td>Gladiator's Glaive</td>
      <td>1.36</td>
    </tr>
    <tr>
      <th>648</th>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
    </tr>
  </tbody>
</table>
</div>



# Other Analysis - Gender Purchase Total %s


```python
pur_analysis_gen.style.format({'Total Purchase Value': '${:.2f}', 'Average Purchase Price': '${:.2f}', 'Normalized Totals': '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_f6235e98_8f69_11e7_8da0_d4619d1535aa" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" ># of Purchases</th> 
        <th class="col_heading level0 col1" >Total Purchase Value</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_f6235e98_8f69_11e7_8da0_d4619d1535aa" class="row_heading level0 row0" >Female</th> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow0_col0" class="data row0 col0" >136</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow0_col1" class="data row0 col1" >$382.91</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow0_col2" class="data row0 col2" >$2.82</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow0_col3" class="data row0 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_f6235e98_8f69_11e7_8da0_d4619d1535aa" class="row_heading level0 row1" >Male</th> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow1_col0" class="data row1 col0" >633</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow1_col1" class="data row1 col1" >$1867.68</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow1_col2" class="data row1 col2" >$2.95</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow1_col3" class="data row1 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_f6235e98_8f69_11e7_8da0_d4619d1535aa" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow2_col0" class="data row2 col0" >11</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow2_col1" class="data row2 col1" >$35.74</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow2_col2" class="data row2 col2" >$3.25</td> 
        <td id="T_f6235e98_8f69_11e7_8da0_d4619d1535aarow2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 




```python
percent_total_gen = pur_analysis_gen['Total Purchase Value']/total_rev
percent_total_gen
```




    Gender
    Female                   0.167478
    Male                     0.816890
    Other / Non-Disclosed    0.015632
    Name: Total Purchase Value, dtype: float64




```python

```
