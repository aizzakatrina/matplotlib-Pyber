# Rideshare Analysis

```python
# dependencies
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import os
```


```python
# read csv
path_city = os.path.join('city_data.csv')
path_ride = os.path.join('ride_data.csv')

df_city = pd.read_csv(path_city)
df_ride = pd.read_csv(path_ride)

# merge dataframes
df_merged = pd.merge(df_city, df_ride, on='city')

# rearrange columns
col = [0,3,4,5,1,2]
df_city_ride = df_merged.iloc[:,col]

df_city_ride.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create group based on the values in the "city column
group_city = df_city_ride.groupby(['city'])

# Calculate Average Fare ($) Per City
average_fare = group_city['fare'].mean()

# Calculate Total Number of Rides Per City
total_rides = group_city['ride_id'].nunique()

# Calculate Total Number of Drivers Per City
total_drivers = group_city['driver_count'].max()

# Determine City Type 
city_type = group_city['type'].max()

# Create dataframe with results
summary_city = pd.DataFrame({'Average Fare ($)': average_fare,
                          'Total Number of Rides': total_rides,
                          'Total Number of Drivers': total_drivers,
                          'City Type': city_type})
# Group by City Type
# Urban
urban = summary_city.loc[summary_city['City Type'] == 'Urban']
total_rides_urban = urban['Total Number of Rides']
avg_fare_urban = urban['Average Fare ($)']
total_drivers_urban = urban['Total Number of Drivers']

# Suburban
suburban = summary_city.loc[summary_city['City Type'] == 'Suburban']
total_rides_suburban = suburban['Total Number of Rides']
avg_fare_suburban = suburban['Average Fare ($)']
total_drivers_suburban = suburban['Total Number of Drivers']

# Rural
rural = summary_city.loc[summary_city['City Type'] == 'Rural']
total_rides_rural = rural['Total Number of Rides']
avg_fare_rural = rural['Average Fare ($)']
total_drivers_rural = rural['Total Number of Drivers']

summary_city.head()
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
      <th>Average Fare ($)</th>
      <th>City Type</th>
      <th>Total Number of Drivers</th>
      <th>Total Number of Rides</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.928710</td>
      <td>Urban</td>
      <td>21</td>
      <td>31</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>Urban</td>
      <td>67</td>
      <td>26</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.315556</td>
      <td>Suburban</td>
      <td>16</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>Urban</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>Urban</td>
      <td>49</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
# use the scatter function
plt.figure()
urban_plot = plt.scatter(total_rides_urban, avg_fare_urban, s=total_drivers_urban*10, 
                         color='lightcoral', alpha=0.5, edgecolor='black', linewidths=1.5, 
                         label='Urban')
suburban_plot = plt.scatter(total_rides_suburban, avg_fare_suburban, s=total_drivers_suburban*10, 
                            color='lightskyblue', alpha=0.5, edgecolor='black',linewidths=1.5, 
                            label='Suburban')
rural_plot = plt.scatter(total_rides_rural, avg_fare_rural, s=total_drivers_rural*10, 
                         color='gold', alpha=0.5, edgecolor='black',linewidths=1.5, 
                         label='Rural')

# label chart
plt.title('Pyber Ride Sharing Data (2016)')
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')

# add and format legend
lgnd = plt.legend(handles=[urban_plot,suburban_plot,rural_plot], loc ='upper right')
lgnd.legendHandles[0]._sizes = [20]
lgnd.legendHandles[1]._sizes = [20]
lgnd.legendHandles[2]._sizes = [20]

plt.show()
```


![png](output_3_0.png)



```python
# % of Total Fares by City Type
# Group by city type
group_type = df_city_ride.groupby(['type'])

# Calculate total fare per city type
total_fare_all = group_type['fare'].sum()
total_fare_df = pd.DataFrame({'Total Fare Per Type': total_fare_all})

# Calculate percent of each city type
percent_f_per_type = total_fare_df['Total Fare Per Type'] / total_fare_df['Total Fare Per Type'].sum() * 100 
percentages_fare_df = pd.DataFrame({'Percentage': percent_f_per_type})

# Summarize
percent_fare_type = pd.merge(total_fare_df, percentages_fare_df, right_index=True, left_index=True)

percent_f_rural = percent_fare_type.iloc[0][1]
percent_f_suburban = percent_fare_type.iloc[1][1]
percent_f_urban = percent_fare_type.iloc[2][1]

# User bar chart
fexplode = [0.1, 0.1, 0]
flabels = ['Rural', 'Suburban', 'Urban']
percents_fare = [percent_f_rural, percent_f_suburban, percent_f_urban]
fcolors = ['gold', 'lightskyblue', 'lightcoral']

plt.pie(percents_fare, labels=flabels, colors=fcolors, explode=fexplode, 
        autopct="{0:1.1f}%".format, shadow=True, startangle=140)
plt.title('% of Total Fares by City Type')
plt.axis("equal")

plt.show()
percent_fare_type.head()
```


![png](output_4_0.png)





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
      <th>Total Fare Per Type</th>
      <th>Percentage</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
      <td>6.579786</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
      <td>31.445750</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
      <td>61.974463</td>
    </tr>
  </tbody>
</table>
</div>




```python
# % of Total Rides by City Type

# Calculate total rides per city type
total_rides_all = group_type['ride_id'].nunique()
total_rides_df = pd.DataFrame({'Total Rides Per Type': total_rides_all})

# Calculate percent of each city type
percent_r_per_type = total_rides_df['Total Rides Per Type'] / total_rides_df['Total Rides Per Type'].sum() * 100 
percentages_rides_df = pd.DataFrame({'Percentage': percent_r_per_type})

# Summarize
percent_rides_type = pd.merge(total_rides_df, percentages_rides_df, right_index=True, left_index=True)

percent_r_rural = percent_rides_type.iloc[0][1]
percent_r_suburban = percent_rides_type.iloc[1][1]
percent_r_urban = percent_rides_type.iloc[2][1]

# User bar chart
rexplode = [0.1, 0.1, 0]
rlabels = ['Rural', 'Suburban', 'Urban']
percents_rides = [percent_r_rural, percent_r_suburban, percent_r_urban]
rcolors = ['gold', 'lightskyblue', 'lightcoral']

plt.pie(percents_rides, labels=rlabels, colors=rcolors, explode=rexplode, 
        autopct="{0:1.1f}%".format, shadow=True, startangle=140)
plt.title('% of Total Rides by City Type')
plt.axis("equal")

plt.show()
percent_rides_type.head()
```


![png](output_5_0.png)





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
      <th>Total Rides Per Type</th>
      <th>Percentage</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>5.263158</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>625</td>
      <td>26.315789</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
      <td>68.421053</td>
    </tr>
  </tbody>
</table>
</div>




```python
# % of Total Drivers by City Type

# Calculate total drivers and total drivers by city type
urban_d = summary_city.loc[summary_city['City Type']=='Urban']
total_urban_d = urban_d['Total Number of Drivers'].sum()

suburban_d = summary_city.loc[summary_city['City Type']=='Suburban']
total_suburban_d = suburban_d['Total Number of Drivers'].sum()

rural_d = summary_city.loc[summary_city['City Type']=='Rural']
total_rural_d = rural_d['Total Number of Drivers'].sum()

total_drivers_all = summary_city['Total Number of Drivers'].sum()

# Calculate percent of each city type
percent_urban_d = total_urban_d / total_drivers_all * 100
percent_suburban_d = total_suburban_d / total_drivers_all * 100
percent_rural_d = total_rural_d / total_drivers_all * 100

# User bar chart
dexplode = [0.1, 0.1, 0]
dlabels = ['Rural', 'Suburban', 'Urban']
percents_drivers = [percent_rural_d, percent_suburban_d, percent_urban_d]
dcolors = ['gold', 'lightskyblue', 'lightcoral']

plt.pie(percents_drivers, labels=dlabels, colors=dcolors, explode=dexplode, 
        autopct="{0:1.1f}%".format, shadow=True, startangle=140)
plt.title('% of Total Drivers by City Type')
plt.axis("equal")

plt.show()
```


![png](output_6_0.png)

