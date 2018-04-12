
# Pyber Ride Sharing

## Observation 1:  The more densely populated areas have more drivers, but a proportionately lower amount of rides, and have an even lower total % of total fares.
## Observation 2: The urban areas are more densely saturated with drivers as they provide a much higher average fare.  This might be due to the fact that the trip lengths in the city are longer.
## Observation 3:  Based on the data, I would recommend targeting urban areas for future expansion.  The average fare is higher, and the total fares make up a proportionately lower amount than the other city types.  This signifies that the market saturation is still at a good spot for further expansion.
## Observation 4 (bonus):  WIth just over 60k in total fares for 2016, it seems like this ride share company would be hard pressed to hire a Data Scientist!


```python
# Dependencies
import pandas as pd
import matplotlib.pyplot as plt
```


```python
#import data and store it in a dataframe
city_file = "raw_data/city_data.csv"
ride_file = "raw_data/ride_data.csv"

city_df = pd.read_csv(city_file)
ride_df = pd.read_csv(ride_file)
```


```python
#print the first few rows of the city df
city_df.head()
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
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#rename columns for a merge later
city_df = city_df.rename(columns={"city":"City", "driver_count":"Driver Count", "type":"Type"})
city_df.head()
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
      <th>City</th>
      <th>Driver Count</th>
      <th>Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#print the first few rows of the ride df
ride_df.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
#clean up the ride_df
clean_ride_df = ride_df.drop(columns=["ride_id"])
clean_ride_df.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
    </tr>
  </tbody>
</table>
</div>




```python
#group the ride_df by city for further analysis and rename the fare column
grouped_ride_df = clean_ride_df.groupby(clean_ride_df["city"], as_index=False)
average_fare = grouped_ride_df.mean()
average_fare = average_fare.rename(columns={"fare":"Average Fare", "city":"City"})
average_fare.head()
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
      <th>City</th>
      <th>Average Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>23.928710</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>20.609615</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>37.315556</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>23.625000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>21.981579</td>
    </tr>
  </tbody>
</table>
</div>




```python
#calculate fare amount by city and total fares and rename the fare column
city_fare_amount = grouped_ride_df.sum()
city_fare_amount = city_fare_amount.rename(columns={"city":"City","fare":"Total Fares"})
city_fare_amount.head()
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
      <th>City</th>
      <th>Total Fares</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>741.79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>535.85</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>335.84</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>519.75</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>417.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
#calculate ride count by city and the total ride count and rename columns appropriately
city_ride_count = clean_ride_df["city"].value_counts()
city_ride_count_df = pd.DataFrame.from_dict(city_ride_count)
city_ride_count_df = city_ride_count_df.reset_index().rename(columns={"index":"City", "city":"Ride Count"})
city_ride_count_df.head()
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
      <th>City</th>
      <th>Ride Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Swansonbury</td>
      <td>34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Port Johnstad</td>
      <td>34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port James</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>South Louis</td>
      <td>32</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jacobfort</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create a new df and group by city type
new_df = pd.merge(city_df, average_fare, on="City")
combined_df = pd.merge(new_df, city_fare_amount, on="City")
combined_df = pd.merge(combined_df, city_ride_count_df, on="City")
grouped_city_type = combined_df.groupby(combined_df["Type"], as_index=False)
total_city_type = grouped_city_type.sum()
total_city_type["Type"] = ["Rural", "Suburban", "Urban"]
total_city_type
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
      <th>Driver Count</th>
      <th>Average Fare</th>
      <th>Total Fares</th>
      <th>Ride Count</th>
      <th>Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>104</td>
      <td>615.728572</td>
      <td>4255.09</td>
      <td>125</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>1</th>
      <td>638</td>
      <td>1300.433953</td>
      <td>20335.69</td>
      <td>657</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2607</td>
      <td>1623.863390</td>
      <td>40078.34</td>
      <td>1625</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>



# Bubble Plot of Rideshare Data


```python
#create separate dataframes based on the 3 different city types
urban_data = combined_df.loc[combined_df["Type"] == "Urban"]
suburban_data = combined_df.loc[combined_df["Type"] == "Suburban"]
rural_data = combined_df.loc[combined_df["Type"] == "Rural"]
```


```python
#Plot our data on a scatter plot
urban = plt.scatter(urban_data["Ride Count"], urban_data["Average Fare"], marker="o", facecolors ="lightskyblue", edgecolors="black",
            s=urban_data["Driver Count"]*10, alpha=0.6, linewidths=1.5, label = "Urban")
suburban = plt.scatter(suburban_data["Ride Count"], suburban_data["Average Fare"], marker="o", facecolors ="lightcoral", edgecolors="black",
            s=suburban_data["Driver Count"]*10, alpha=0.6, linewidths=1.5, label = "Suburban")
rural = plt.scatter(rural_data["Ride Count"], rural_data["Average Fare"], marker="o", facecolors ="gold", edgecolors="black",
            s=rural_data["Driver Count"]*10, alpha=0.6, linewidths=1.5, label = "Rural")

#set up the legend for the scatter plot
legend = plt.legend(handles=[urban, suburban, rural])
legend.legendHandles[0]._sizes = [80]
legend.legendHandles[1]._sizes = [80]
legend.legendHandles[2]._sizes = [80]

#add in a chart title, axis titles, and notes
text = "Note: Circle size correlates with count of drivers per city."
plt.text(38,35,text)
plt.xlabel("Average Fare ($)")
plt.ylabel("Total Number of Rides (Per City)")
plt.title("Pyber Ride Sharing Data (2016)")
```




    Text(0.5,1,'Pyber Ride Sharing Data (2016)')




![png](output_14_1.png)


# Total Fares by City Type


```python
#create a pie chart for % of Total Fares by City Type

# Labels for the sections of our pie chart
labels = total_city_type["Type"]

# The values of each section of the pie chart
sizes = total_city_type["Total Fares"]

# The colors of each section of the pie chart
colors = ["gold","lightcoral", "lightskyblue"]

# Tells matplotlib to seperate the "Urban" section from the others
explode = (0, 0, 0.1)

#plot our data on a pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=150)

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

# give our plot a title
plt.title("% of Total Fares by City Type")
```




    Text(0.5,1,'% of Total Fares by City Type')




![png](output_16_1.png)


# Total Rides by City Type


```python
#create a pie chart for % of Total Rides by City Type

# Labels for the sections of our pie chart
labels = total_city_type["Type"]

# The values of each section of the pie chart
sizes = total_city_type["Ride Count"]

# The colors of each section of the pie chart
colors = ["gold","lightcoral", "lightskyblue"]

# Tells matplotlib to seperate the "Urban" section from the others
explode = (0, 0, 0.1)

#plot our data on a pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=150)

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

# give our chart a title
plt.title("% of Total Rides by City Type")
```




    Text(0.5,1,'% of Total Rides by City Type')




![png](output_18_1.png)


# Total Drivers by City Type


```python
# create a pie chart for % of Total Drivers by City Type

# Labels for the sections of our pie chart
labels = total_city_type["Type"]

# The values of each section of the pie chart
sizes = total_city_type["Driver Count"]

# The colors of each section of the pie chart
colors = ["gold","lightcoral", "lightskyblue"]

# Tells matplotlib to seperate the "Urban" section from the others
explode = (0, 0, 0.1)

#plot our data on a pie chart
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=150)

# Tells matplotlib that we want a pie chart with equal axes
plt.axis("equal")

#give our chart a title
plt.title("% of Total Drivers by City Type")
```




    Text(0.5,1,'% of Total Drivers by City Type')




![png](output_20_1.png)

