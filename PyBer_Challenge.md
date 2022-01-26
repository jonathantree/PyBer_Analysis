# Pyber Challenge

### 4.3 Loading and Reading CSV files


```python
# Add Matplotlib inline magic command
%matplotlib inline
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import os
import numpy as np
import tabulate
# File to Load (Remember to change these)

city_data_to_load = os.path.join("Resources", "city_data.csv")
ride_data_to_load = os.path.join("Resources", "ride_data.csv")


# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)
```

### Merge the DataFrames


```python
# Combine the data into a single dataset
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])

# Display the data table for preview
pyber_data_df.head()
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
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2019-01-14 10:14:22</td>
      <td>13.83</td>
      <td>5739410935873</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Michelleport</td>
      <td>2019-03-04 18:24:09</td>
      <td>30.24</td>
      <td>2343912425577</td>
      <td>72</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Samanthamouth</td>
      <td>2019-02-24 04:29:00</td>
      <td>33.44</td>
      <td>2005065760003</td>
      <td>57</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>2019-02-10 23:22:03</td>
      <td>23.44</td>
      <td>5149245426178</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South Jack</td>
      <td>2019-03-06 04:28:35</td>
      <td>34.58</td>
      <td>3908451377344</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>



## Challenge Deliverable 1. Generate a Ride-Sharing DataFrame by City Type


```python
#  1. Get the total rides for each city type
ride_count = pyber_data_df.groupby(["type"]).count()["ride_id"]
print(ride_count)
```

    type
    Rural        125
    Suburban     625
    Urban       1625
    Name: ride_id, dtype: int64
    


```python
# 2. Get the total drivers for each city type
total_driver_count = city_data_df.groupby(["type"]).sum()["driver_count"]
print(total_driver_count)
```

    type
    Rural         78
    Suburban     490
    Urban       2405
    Name: driver_count, dtype: int64
    


```python
#  3. Get the total amount of fares for each city type
fares = pyber_data_df.groupby(["type"]).sum()["fare"]
print(fares)
```

    type
    Rural        4327.93
    Suburban    19356.33
    Urban       39854.38
    Name: fare, dtype: float64
    


```python
#  4. Get the average fare per ride for each city type. 
avg_fare_per_ride = fares.divide(ride_count)
print(avg_fare_per_ride)
```

    type
    Rural       34.623440
    Suburban    30.970128
    Urban       24.525772
    dtype: float64
    


```python
# 5. Get the average fare per driver for each city type. 
avg_fare_per_driver = fares.divide(total_driver_count)
print(avg_fare_per_driver)
```

    type
    Rural       55.486282
    Suburban    39.502714
    Urban       16.571468
    dtype: float64
    


```python
#  6. Create a PyBer summary DataFrame. 
pyber_summary_df = pd.DataFrame({
    "Total Rides" : ride_count,
    "Total Drivers" : total_driver_count,
    "Total Fares" : fares,
    "Average Fare per Ride" : avg_fare_per_ride,
    "Average Fare per Driver" : avg_fare_per_driver
})

pyber_summary_df
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
      <th>Total Rides</th>
      <th>Total Drivers</th>
      <th>Total Fares</th>
      <th>Average Fare per Ride</th>
      <th>Average Fare per Driver</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>78</td>
      <td>4327.93</td>
      <td>34.623440</td>
      <td>55.486282</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>625</td>
      <td>490</td>
      <td>19356.33</td>
      <td>30.970128</td>
      <td>39.502714</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
      <td>2405</td>
      <td>39854.38</td>
      <td>24.525772</td>
      <td>16.571468</td>
    </tr>
  </tbody>
</table>
</div>




```python
#  7. Cleaning up the DataFrame. Delete the index name
pyber_summary_df.index.name = None
```


```python
#  8. Format the columns.
pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map("${:,.2f}".format)
pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map("${:.2f}".format)
pyber_summary_df["Average Fare per Driver"] = pyber_summary_df["Average Fare per Driver"].map("${:.2f}".format)

pyber_summary_df
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
      <th>Total Rides</th>
      <th>Total Drivers</th>
      <th>Total Fares</th>
      <th>Average Fare per Ride</th>
      <th>Average Fare per Driver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>78</td>
      <td>$4,327.93</td>
      <td>$34.62</td>
      <td>$55.49</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>625</td>
      <td>490</td>
      <td>$19,356.33</td>
      <td>$30.97</td>
      <td>$39.50</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
      <td>2405</td>
      <td>$39,854.38</td>
      <td>$24.53</td>
      <td>$16.57</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(pyber_summary_df.head().to_markdown())
```

    |          |   Total Rides |   Total Drivers | Total Fares   | Average Fare per Ride   | Average Fare per Driver   |
    |:---------|--------------:|----------------:|:--------------|:------------------------|:--------------------------|
    | Rural    |           125 |              78 | $4,327.93     | $34.62                  | $55.49                    |
    | Suburban |           625 |             490 | $19,356.33    | $30.97                  | $39.50                    |
    | Urban    |          1625 |            2405 | $39,854.38    | $24.53                  | $16.57                    |
    

## Deliverable 2.  Create a multiple line plot that shows the total weekly of the fares for each type of city.


```python
# Print the merged DataFrame for reference.
pyber_data_df.head()
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
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lake Jonathanshire</td>
      <td>2019-01-14 10:14:22</td>
      <td>13.83</td>
      <td>5739410935873</td>
      <td>5</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Michelleport</td>
      <td>2019-03-04 18:24:09</td>
      <td>30.24</td>
      <td>2343912425577</td>
      <td>72</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Samanthamouth</td>
      <td>2019-02-24 04:29:00</td>
      <td>33.44</td>
      <td>2005065760003</td>
      <td>57</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rodneyfort</td>
      <td>2019-02-10 23:22:03</td>
      <td>23.44</td>
      <td>5149245426178</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South Jack</td>
      <td>2019-03-06 04:28:35</td>
      <td>34.58</td>
      <td>3908451377344</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 1. Using groupby() to create a new DataFrame showing the sum of the fares 
#  for each date where the indices are the city type and date.
fares_sum_per_date = pyber_data_df.groupby(["type", "date"]).sum()["fare"]
fares_sum_per_date
```




    type   date               
    Rural  2019-01-01 09:45:36    43.69
           2019-01-02 11:18:32    52.12
           2019-01-03 19:51:01    19.90
           2019-01-04 03:31:26    24.88
           2019-01-06 07:38:40    47.33
                                  ...  
    Urban  2019-05-08 04:20:00    21.99
           2019-05-08 04:39:49    18.45
           2019-05-08 07:29:01    18.55
           2019-05-08 11:38:35    19.77
           2019-05-08 13:10:18    18.04
    Name: fare, Length: 2375, dtype: float64




```python
fares_sum_per_date_df = pd.DataFrame(fares_sum_per_date)
fares_sum_per_date_df
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
      <th>fare</th>
    </tr>
    <tr>
      <th>type</th>
      <th>date</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Rural</th>
      <th>2019-01-01 09:45:36</th>
      <td>43.69</td>
    </tr>
    <tr>
      <th>2019-01-02 11:18:32</th>
      <td>52.12</td>
    </tr>
    <tr>
      <th>2019-01-03 19:51:01</th>
      <td>19.90</td>
    </tr>
    <tr>
      <th>2019-01-04 03:31:26</th>
      <td>24.88</td>
    </tr>
    <tr>
      <th>2019-01-06 07:38:40</th>
      <td>47.33</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">Urban</th>
      <th>2019-05-08 04:20:00</th>
      <td>21.99</td>
    </tr>
    <tr>
      <th>2019-05-08 04:39:49</th>
      <td>18.45</td>
    </tr>
    <tr>
      <th>2019-05-08 07:29:01</th>
      <td>18.55</td>
    </tr>
    <tr>
      <th>2019-05-08 11:38:35</th>
      <td>19.77</td>
    </tr>
    <tr>
      <th>2019-05-08 13:10:18</th>
      <td>18.04</td>
    </tr>
  </tbody>
</table>
<p>2375 rows × 1 columns</p>
</div>




```python
# 2. Reset the index on the DataFrame you created in #1. This is needed to use the 'pivot()' function.
fares_sum_per_date_df = fares_sum_per_date_df.reset_index()
fares_sum_per_date_df
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
      <th>type</th>
      <th>date</th>
      <th>fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>2019-01-01 09:45:36</td>
      <td>43.69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Rural</td>
      <td>2019-01-02 11:18:32</td>
      <td>52.12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rural</td>
      <td>2019-01-03 19:51:01</td>
      <td>19.90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rural</td>
      <td>2019-01-04 03:31:26</td>
      <td>24.88</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rural</td>
      <td>2019-01-06 07:38:40</td>
      <td>47.33</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2370</th>
      <td>Urban</td>
      <td>2019-05-08 04:20:00</td>
      <td>21.99</td>
    </tr>
    <tr>
      <th>2371</th>
      <td>Urban</td>
      <td>2019-05-08 04:39:49</td>
      <td>18.45</td>
    </tr>
    <tr>
      <th>2372</th>
      <td>Urban</td>
      <td>2019-05-08 07:29:01</td>
      <td>18.55</td>
    </tr>
    <tr>
      <th>2373</th>
      <td>Urban</td>
      <td>2019-05-08 11:38:35</td>
      <td>19.77</td>
    </tr>
    <tr>
      <th>2374</th>
      <td>Urban</td>
      <td>2019-05-08 13:10:18</td>
      <td>18.04</td>
    </tr>
  </tbody>
</table>
<p>2375 rows × 3 columns</p>
</div>




```python
# 3. Create a pivot table with the 'date' as the index, the columns ='type', and values='fare' 
# to get the total fares for each type of city by the date. 
date_fares_pivot = fares_sum_per_date_df.pivot(index="date", columns="type", values="fare")
date_fares_pivot
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
      <th>type</th>
      <th>Rural</th>
      <th>Suburban</th>
      <th>Urban</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 00:08:16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>37.91</td>
    </tr>
    <tr>
      <th>2019-01-01 00:46:46</th>
      <td>NaN</td>
      <td>47.74</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 02:07:24</th>
      <td>NaN</td>
      <td>24.07</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 03:46:50</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.57</td>
    </tr>
    <tr>
      <th>2019-01-01 05:23:21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.75</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-05-08 04:20:00</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>21.99</td>
    </tr>
    <tr>
      <th>2019-05-08 04:39:49</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.45</td>
    </tr>
    <tr>
      <th>2019-05-08 07:29:01</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.55</td>
    </tr>
    <tr>
      <th>2019-05-08 11:38:35</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.77</td>
    </tr>
    <tr>
      <th>2019-05-08 13:10:18</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>18.04</td>
    </tr>
  </tbody>
</table>
<p>2375 rows × 3 columns</p>
</div>




```python
# 4. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-28'.
jantoapr_df = date_fares_pivot.loc['2019-01-01':'2019-04-28']
jantoapr_df
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
      <th>type</th>
      <th>Rural</th>
      <th>Suburban</th>
      <th>Urban</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 00:08:16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>37.91</td>
    </tr>
    <tr>
      <th>2019-01-01 00:46:46</th>
      <td>NaN</td>
      <td>47.74</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 02:07:24</th>
      <td>NaN</td>
      <td>24.07</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 03:46:50</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.57</td>
    </tr>
    <tr>
      <th>2019-01-01 05:23:21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.75</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-04-27 17:58:27</th>
      <td>14.01</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-04-27 19:45:48</th>
      <td>NaN</td>
      <td>28.84</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-04-27 20:41:36</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.28</td>
    </tr>
    <tr>
      <th>2019-04-27 23:26:03</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.06</td>
    </tr>
    <tr>
      <th>2019-04-27 23:52:44</th>
      <td>NaN</td>
      <td>45.98</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>2177 rows × 3 columns</p>
</div>




```python
# 5. Set the "date" index to datetime datatype. This is necessary to use the resample() method in Step 8.
jantoapr_df.index = pd.to_datetime(jantoapr_df.index)
jantoapr_df
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
      <th>type</th>
      <th>Rural</th>
      <th>Suburban</th>
      <th>Urban</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-01 00:08:16</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>37.91</td>
    </tr>
    <tr>
      <th>2019-01-01 00:46:46</th>
      <td>NaN</td>
      <td>47.74</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 02:07:24</th>
      <td>NaN</td>
      <td>24.07</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-01-01 03:46:50</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.57</td>
    </tr>
    <tr>
      <th>2019-01-01 05:23:21</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.75</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2019-04-27 17:58:27</th>
      <td>14.01</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-04-27 19:45:48</th>
      <td>NaN</td>
      <td>28.84</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-04-27 20:41:36</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.28</td>
    </tr>
    <tr>
      <th>2019-04-27 23:26:03</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>19.06</td>
    </tr>
    <tr>
      <th>2019-04-27 23:52:44</th>
      <td>NaN</td>
      <td>45.98</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>2177 rows × 3 columns</p>
</div>




```python
# 6. Check that the datatype for the index is datetime using df.info()
jantoapr_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    DatetimeIndex: 2177 entries, 2019-01-01 00:08:16 to 2019-04-27 23:52:44
    Data columns (total 3 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   Rural     114 non-null    float64
     1   Suburban  567 non-null    float64
     2   Urban     1496 non-null   float64
    dtypes: float64(3)
    memory usage: 68.0 KB
    


```python
# 7. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
weeks_df = jantoapr_df.resample('W').sum()
weeks_df
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
      <th>type</th>
      <th>Rural</th>
      <th>Suburban</th>
      <th>Urban</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01-06</th>
      <td>187.92</td>
      <td>721.60</td>
      <td>1661.68</td>
    </tr>
    <tr>
      <th>2019-01-13</th>
      <td>67.65</td>
      <td>1105.13</td>
      <td>2050.43</td>
    </tr>
    <tr>
      <th>2019-01-20</th>
      <td>306.00</td>
      <td>1218.20</td>
      <td>1939.02</td>
    </tr>
    <tr>
      <th>2019-01-27</th>
      <td>179.69</td>
      <td>1203.28</td>
      <td>2129.51</td>
    </tr>
    <tr>
      <th>2019-02-03</th>
      <td>333.08</td>
      <td>1042.79</td>
      <td>2086.94</td>
    </tr>
    <tr>
      <th>2019-02-10</th>
      <td>115.80</td>
      <td>974.34</td>
      <td>2162.64</td>
    </tr>
    <tr>
      <th>2019-02-17</th>
      <td>95.82</td>
      <td>1045.50</td>
      <td>2235.07</td>
    </tr>
    <tr>
      <th>2019-02-24</th>
      <td>419.06</td>
      <td>1412.74</td>
      <td>2466.29</td>
    </tr>
    <tr>
      <th>2019-03-03</th>
      <td>175.14</td>
      <td>858.46</td>
      <td>2218.20</td>
    </tr>
    <tr>
      <th>2019-03-10</th>
      <td>303.94</td>
      <td>925.27</td>
      <td>2470.93</td>
    </tr>
    <tr>
      <th>2019-03-17</th>
      <td>163.39</td>
      <td>906.20</td>
      <td>2044.42</td>
    </tr>
    <tr>
      <th>2019-03-24</th>
      <td>189.76</td>
      <td>1122.20</td>
      <td>2368.37</td>
    </tr>
    <tr>
      <th>2019-03-31</th>
      <td>199.42</td>
      <td>1045.06</td>
      <td>1942.77</td>
    </tr>
    <tr>
      <th>2019-04-07</th>
      <td>501.24</td>
      <td>1010.73</td>
      <td>2356.70</td>
    </tr>
    <tr>
      <th>2019-04-14</th>
      <td>269.79</td>
      <td>784.82</td>
      <td>2390.72</td>
    </tr>
    <tr>
      <th>2019-04-21</th>
      <td>214.14</td>
      <td>1149.27</td>
      <td>2303.80</td>
    </tr>
    <tr>
      <th>2019-04-28</th>
      <td>191.85</td>
      <td>1169.04</td>
      <td>1909.51</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 8. Using the object-oriented interface method, plot the resample DataFrame using the df.plot() function. 

# Import the style from Matplotlib.
from matplotlib import style
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')

fig = plt.figure()
ax = weeks_df.plot(figsize = (20,5))

plt.title("Total Fare by City Type", fontsize=20)
plt.ylabel("Fare ($USD)", fontsize=12)
plt.xlabel("")

lgnd = ax.legend(fontsize="12", mode="Expanded", title="City Types", ncol=3, loc='lower center',
                 bbox_to_anchor=(0.5, -0.5))
lgnd.get_title().set_fontsize(14)


plt.tight_layout()
plt.savefig("Analysis/PyBer_fare_summary.png")

```


    <Figure size 432x288 with 0 Axes>



    
![png](PyBer_Challenge_files/PyBer_Challenge_25_1.png)
    



```python

```
