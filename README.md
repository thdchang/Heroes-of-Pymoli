
# Heroes Of Pymoli Data Analysis


Observable Trends
1. The total purchase revenue data broken down into age categories closely resembles a normal distribution with a peak at the age group 20-24. Within the age categories, 20-24 year olds are the source of highest revenue and item purchase counts.

2. Average purchase price for each top 5 spenders range from $3.18 to $4.24. Compared to the entire dataset's average purchase price ($2.93), the top spenders are more likely to purchase the more expensive items. 

3. Most profitable items are relatively the most expensive items in the game. High purchase counts for these items also indicate the item's desirability. It can be induced that these expensive items probably enhances the gameplay. However, without additional features for each item such as attack stats, this analysis is not strong.


```python
#modules used in this exercise 
import pandas as pd 
import json

#read json data file
json_data = open('purchase_data.json').read()
data = json.loads(json_data) 

#construct dataframe of converted json file
data_df = pd.DataFrame(data)

```

# Player Count


```python
#---------------------------------------------------------

#Below:     Find the player count and create a dataframe

#------------------------------

#finding the unique players in the dataset
unique_player_list = data_df["SN"].value_counts()

#length of array of unique player list
player_count = len(unique_player_list)

#to create a data frame to display player count create column 
#and name the column to be used to create a dataframe
player_count_series = pd.Series(player_count)
player_count_series.name = "Total Unqiue Players"

#construct and display dataframe
player_count_df = pd.DataFrame(player_count_series)
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
      <th>Total Unqiue Players</th>
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



# Purchasing Analysis (Total)


```python
#---------------------------------------------------------

#Below: Purchasing Analysis (Total)
#1. Number of Unique Items 
#2. Average Purchase Price 
#3. Total Number of Purchases 
#4. Total Revenue. 

#------------------------------

#unique() method removes duplicate "item id" data values
#to evalute to a list of unique items purchased 
#next len() method evaluates the length of the list
#to calculate the number of unique items 

num_unique_items = len(data_df["Item ID"].unique())


#mean() method used to calculate the mean "price" data values
#of the column "Price" in our dataframe

avg_purchase_price = data_df["Price"].mean()


#len() method calculates the length of the data values 
#in a column of the dataframe to determine 

total_num_purchases = data_df["Item ID"].count()

#sum() method is used to sum up the data values of the 
#"Price" column to calculate the total revenue. 

total_revenue = data_df["Price"].sum()


#dataframe to display:
#(a) Number of Unique Items 
#(b) Average Purchase Price 
#(c) Total Number of Purchases 
#(d) Total Revenue 

#form a dictionary to construct a Purchasing Analysis (Total) Dataframe
purchasing_analysis_dict = {"Number of Unique Items": [num_unique_items], "Average Purchase Price": [avg_purchase_price],
                           "Total Number of Purchases": [total_num_purchases], "Total Revenue": [total_revenue]}


#pass pd.DataFrame arguments to be used 
purch_analysis_df = pd.DataFrame(purchasing_analysis_dict, columns = ["Number of Unique Items", 
                                                                      "Average Purchase Price", 
                                                                      "Total Number of Purchases", 
                                                                      "Total Revenue"])

#format "Average Purchase Price" and "Total Revenue to two decimal places and add $ sign,
#the float datatype is converted to a string datatype 
purch_analysis_df["Average Purchase Price"] = purch_analysis_df["Average Purchase Price"].map("${:.2f}".format)
purch_analysis_df["Total Revenue"] = purch_analysis_df["Total Revenue"].map("${:.2f}".format)


#display dataframe
purch_analysis_df


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
      <th>Average Purchase Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>



# Gender Demographics


```python
#---------------------------------------------------------

#Gender Demographics 
#1. Percentage and Count of Male Players 
#2. Percentage and Count of Female Players
#3. Percentage and Count of Other/Non-Disclosed 

#------------------------------

#construct dataframe with only "SN" and "Gender"
gender_demo_df = pd.DataFrame(data_df[["SN", "Gender"]])
#drop duplicate purchases by same player
gender_demo_df = gender_demo_df.drop_duplicates(subset = ["SN"], keep="first")
#count number of players of each gender
gender_demo = gender_demo_df["Gender"].value_counts()


#evalutes the sum of genders of unique player list
total_demo_count = gender_demo.sum()
#series variable containing the gender demographic percentage values
percent_players = (gender_demo / total_demo_count) * 100


#get the array representation for gender demographic percentage series 
#and total demographic count series
gender_demo_list = gender_demo.values
percent_players_list = percent_players.values


#create dictionary to be used to construct dataframe 
gender_demo_dict = {"Percentage of Players": percent_players_list, 
                   "Total Count": gender_demo_list}


#gender categories assigned as index values 
gender_demo_index = ["Male", "Female", "Other / Non-Disclosed"]


#construct gender demographics dataframe
gender_demo_df = pd.DataFrame(gender_demo_dict, columns = ["Percentage of Players",
                                          "Total Count"], index = gender_demo_index)


#format percentage of player data values to 2 decimal places with a percentage symbol
#the float data type converted to string data type
gender_demo_df["Percentage of Players"] = gender_demo_df["Percentage of Players"].map("{:.2f}%".format)


#display dataframe
gender_demo_df
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
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Gender)


```python
#--------------------------------------------------------

#Purchasing Analysis (Gender) 
#The below each broken by gender 
#1. Purchase Count 
#2. Average Purchase Price 
#3. Total Purchase Value 
#4. Normalized Totals 

#------------------------------

#groupby() method dataframe to evaluate the total of purchase values by gender
gender_total_purchase_value = data_df[["Gender", "Price"]].groupby(["Gender"]).sum()

#create total purchase value series with gender index from previous dataframe
gender_total_purchase_value = gender_total_purchase_value["Price"]


#groupby() method dataframe to evaluate the total purchase count by gender
gender_purchase_count = data_df[["Gender", "Price"]].groupby(["Gender"]).count()

#create total purchase count series with gender index from previous dataframe
gender_purchase_count = gender_purchase_count["Price"]


#groupby() method dataframe to evaluate the average purchase price by gender
gender_average_price = data_df[["Gender", "Price"]].groupby(["Gender"]).mean()

#create average purchase price series with gender index from previous dataframe
gender_average_price = gender_average_price["Price"]


#calculate the normalized total for each gender
#by divding total purchase value with its respective total gender demographic count
normalized_total = gender_total_purchase_value.div(gender_demo_df["Total Count"])


#create dictionary to be used to construct dataframe
gender_purchasing_analysis = {"Purchase Count": gender_purchase_count,
                             "Average Purchase Price": gender_average_price,
                             "Total Purchase Value": gender_total_purchase_value,
                             "Normalized Totals": normalized_total}


#construct Purchasing Analysis (Gender) dataframe
gender_purchasing_df = pd.DataFrame(gender_purchasing_analysis, columns = ["Purchase Count", 
                                                   "Average Purchase Price",
                                                   "Total Purchase Value",
                                                   "Normalized Totals"])


#format "Average Purhcase Price", "Total Purchase Value", "Normalized Totals"
#data values to 2 decimal places with a dollar sign
#the float data types converted to string data type
gender_purchasing_df["Average Purchase Price"] = gender_purchasing_df["Average Purchase Price"].map("${:.2f}".format)
gender_purchasing_df["Total Purchase Value"] = gender_purchasing_df["Total Purchase Value"].map("${:.2f}".format)
gender_purchasing_df["Normalized Totals"] = gender_purchasing_df["Normalized Totals"].map("${:.2f}".format)


#display Purchasing Analysis (Gender) dataframe 
gender_purchasing_df
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
      <th>Normalized Totals</th>
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
      <td>$1867.68</td>
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



# Age Demographics


```python
#--------------------------------------------------------

#Age Demographics broken down into categories: 
#1. Percentage of Unique Players in each age category
#2. Total count of Unique Players in each age category
#**the following data is used to calculate the normalized total 
#for next section: Purchasing Analysis (Age) 

#------------------------------



#create dataframe with only "SN" and "Age" features and drop rows with duplicate "SN"
age_demo_df = data_df[["SN", "Age"]].drop_duplicates(subset = "SN", keep="first")


#create bins which data will be held
#range for each bin: start number is exclusive, end number is inclusive
#i.e.) 1st bin is (0,9]
bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]

#index values for each bin
index_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34",
              "35-39", "40+"]

#each "Age" column data values located within it's corresponding bin's range 
#is converted to bin's label; the manipulated data values becomes the 
#index of dataframe named "Age Category"
age_demo_df["Age Category"] = pd.cut(age_demo_df["Age"], bins, labels = index_names)


#count() method counts the number of players in each category
age_demo_df = age_demo_df.groupby("Age Category").count()


#assign "SN" column containing player count in each "Age Category" into an array
age_category_total_count = age_demo_df["SN"]


#evaluate and assign new variable the percentage of each age category
age_category_percent = (age_category_total_count / player_count) * 100
#add "Percentage of Players" column to dataframe
age_demo_df["Percentage of Players"] = age_category_percent


#rename columns describing what the data values are accurately representing
age_demo_df.rename(columns = {"Age": "Total Count"}, inplace=True)


#delete irrelevant column "SN" from dataframe
del age_demo_df["SN"]


#format numeric data for "Percentage of Players" data values as percentages
age_demo_df["Percentage of Players"] = age_demo_df["Percentage of Players"].map("{:.2f}%".format)


#display data Frame 
age_demo_df[["Percentage of Players", "Total Count"]]

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
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Age Category</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32%</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20%</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18%</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20%</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71%</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Age)


```python
#--------------------------------------------------------

#Age Demographics 
#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
#1. Purchase Count 
#2. Average Purchase Price 
#3. Total Purchase Value
#4. Normalized Totals 

#------------------------------

#use the same bins and bin labels as the previous section
bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]

index_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34",
              "35-39", "40+"]

#retyped same code to create "Age Category" column for clarity 
data_df["Age Category"] = pd.cut(data_df["Age"], bins, labels = index_names)

#create series of total purchase value for each age category
category_total_purchase = data_df[["Age Category", "Price"]].groupby("Age Category").sum()
category_total_purchase = category_total_purchase["Price"]

#create series of average purchase price for each age categry
category_average_purchase = data_df[["Age Category", "Price"]].groupby("Age Category").mean()
category_average_purchase = category_average_purchase["Price"]

#create series of purchase count for each age category
category_purchase_count = data_df[["Age Category", "Price"]].groupby("Age Category").count()
category_purchase_count = category_purchase_count["Price"]

#calculate the normalized total for each "Age Category" and assign values to array
category_normalized_total = category_total_purchase / age_demo_df["Total Count"]

#average demographic dictionary created to construct dataframe
age_demo_dict = {"Purchase Count": category_purchase_count,
                "Average Purchase Price": category_average_purchase,
                "Total Purchase Value": category_total_purchase,
                "Normalized Totals": category_normalized_total}

#construct purchasing analysis (age) dataframe
age_purchasing_analysis= pd.DataFrame(age_demo_dict, 
                                     columns = ["Purchase Count",
                                               "Average Purchase Price",
                                               "Total Purchase Value",
                                               "Normalized Totals"])

#format select columns data values 
age_purchasing_analysis["Average Purchase Price"] = age_purchasing_analysis["Average Purchase Price"].map("${:.2f}".format)
age_purchasing_analysis["Total Purchase Value"] = age_purchasing_analysis["Total Purchase Value"].map("${:.2f}".format)
age_purchasing_analysis["Normalized Totals"] = age_purchasing_analysis["Normalized Totals"].map("${:.2f}".format)


#display Purchasing Analysis(Age) Dataframe
age_purchasing_analysis

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
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Category</th>
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



# Top Spenders


```python
#--------------------------------------------------------

#Top Spenders
#Identify the top 5 spenders in the game by total purchase value, then list (in a table):
#1. SN 
#2. Purchase Count 
#3. Average Purchase Price 
#4. Total Purchase Value 

#------------------------------

sn_price_df = data_df[["SN", "Price"]]


pd.DataFrame(sn_price_df)



#calculate the total purchase value for each player
sn_sum_purch = sn_price_df.groupby("SN").sum()
sn_sum_purch = sn_sum_purch["Price"]


#calculate the average purchase item price for each player
sn_avg_purch = sn_price_df.groupby("SN").mean()

#assign "Price" column data values into a list variable
sn_avg_purch = sn_avg_purch["Price"]

#calculate the purchase count for each player
sn_purch_count = sn_price_df.groupby("SN").count()
sn_purch_count = sn_purch_count["Price"]



#create top spender's dictionary to be used to construct dataframe
spenders_dict = {"Purchase Count": sn_purch_count,
                "Average Purchase Price": sn_avg_purch, 
                 "Total Purchase Value": sn_sum_purch}

#construct Top Spenders dataframe
spenders_df = pd.DataFrame(spenders_dict, columns = ["Purchase Count",
                                                    "Average Purchase Price",
                                                    "Total Purchase Value"])

#sort rows in descending order; to display players with most purchase count on top
top_spenders_df = spenders_df.sort_values(by=["Total Purchase Value"], ascending=False)

#format select columns to be currency data values 
top_spenders_df["Average Purchase Price"] = top_spenders_df["Average Purchase Price"].map("${:.2f}".format)
top_spenders_df["Total Purchase Value"] = top_spenders_df["Total Purchase Value"].map("${:.2f}".format)


#display top 5 spenders dataframe
pd.DataFrame(top_spenders_df.head())
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



# Most Popular Items


```python
#--------------------------------------------------------

#Most Popular Items 
#Identify the 5 most popular items by purchase count, then list (in a table): 
#1. Item ID 
#2. Item Name 
#3. Purchase Count 
#4. Item Price 
#5. Total Purchase Value 

#------------------------------

#create dataframe with "Item ID" and "Item Name"
popular_df = data_df[["Item ID", "Item Name", "Price"]]


popular_df = popular_df.groupby(["Item ID", "Item Name", "Price"]).count()

popular_purchase_count = data_df[["Item ID", "Price"]]
popular_purchase_count = popular_purchase_count["Item ID"].value_counts().sort_index()

#assign series to an array of purchase count
popular_purchase_count = popular_purchase_count.values

#add purchase count list as column to popular items dataframe
popular_df["Purchase Count"] = popular_purchase_count
popular_df = popular_df.sort_values(by = "Purchase Count", ascending=False)

#create "Item ID" and "Item Name" to be the only indices for dataframe
most_popular_items = popular_df.reset_index(2)
#add new "Total Purchase Value" column with calculated total purchase value to dataframe
most_popular_items["Total Purchase Value"] = most_popular_items["Purchase Count"] * most_popular_items["Price"]


#rename "Price" column to "Item Price" to clearly describe data value
most_popular_items = most_popular_items.rename(columns = {"Price": "Item Price"})

#construct Most Popular Items dataframe
most_popular_items_df = pd.DataFrame(most_popular_items, columns = ["Purchase Count", "Item Price", "Total Purchase Value"])


#format select columns to display data values as currency 
most_popular_items_df["Item Price"] = most_popular_items_df["Item Price"].map("${:.2f}".format)
most_popular_items_df["Total Purchase Value"] = most_popular_items_df["Total Purchase Value"].map("${:.2f}".format)


#display Top 5 Most Popular Items 
most_popular_items_df.head()
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
      <th>Total Purchase Value</th>
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
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
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



# Most Profitable Items


```python
#--------------------------------------------------------

#Most Profitable Items 
#Identify the 5 most profitable items by total purchase value, then list (in a table): 
#1. Item ID
#2. Item Name 
#3. Purchase Count 
#4. Item Price 
#5. Total Purchase Value

#------------------------------

#create new dataframe to manipulate 
profit_df = data_df[["Item ID", "Item Name", "Price"]]

#calculate the total purchase value of each item by "Item ID" and "Item Name" 
item_purchased_sum = profit_df.groupby(["Item ID", "Item Name"]).sum()
item_purchased_sum = item_purchased_sum.rename(columns = {"Price": "Total Purchase Value"})

#find purchase count for each unique item
item_id_count_series = profit_df["Item ID"].value_counts()

#sort the purchase count according to ascending ordered "Item ID" index
#then move "Item ID" index to the right to be a series of the dataframe
item_id_count_series = item_id_count_series.sort_index().reset_index()
del item_id_count_series["index"]


new_profit_df = item_purchased_sum.reset_index()


new_profit_df["Purchase Count"] = item_id_count_series

#sort dataframe by "Total Purchase Value"; dataframe shows Highest total purchase value first
top_profit_df = new_profit_df.sort_values(by="Total Purchase Value", 
                                          ascending=False)

#add new column "Item Price" with calculated item prices to dataframe
top_profit_df["Item Price"] = top_profit_df["Total Purchase Value"] / top_profit_df["Purchase Count"]

#create multiindex for dataframe
top_profit_df = top_profit_df.set_index(["Item ID", "Item Name"])

top_profit_df = pd.DataFrame(top_profit_df, columns = ["Purchase Count", "Item Price", "Total Purchase Value"])


#format select columns to display data values as currency
top_profit_df["Item Price"] = top_profit_df["Item Price"].map("${:.2f}".format)
top_profit_df["Total Purchase Value"] = top_profit_df["Total Purchase Value"].map("${:.2f}".format)

#display Top 5 Most Profitable Items dataframe
top_profit_df.head()
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
      <th>Total Purchase Value</th>
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


