
# Pyber Rideshare Analysis

# Assesment

1. In general, there is significant scope for adding drivers in rural and suburban areas. Total rides and fares have comparitively higher share, while total drivers is low. However, these should be considered along with total rides and average fairs to prioritize growth. Not all cities in these categories may be worth the expenses.
2. There are selective cities (smaller sized bubbles, where larger bubbles are present) that have scope for adding more drivers
3. Port James has an exceptionally high number of rides (64) and failry high average fare (32). This market seems to be a good opportunity to expand and may be a possible opportunity for specialized services, like rideshare.
4. Some markets like Manuelchester need recosideration, especially if there is signifcant cost associated with maintaining these operations (low rides, minimal drivers)


```python
# Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns
```


```python
#import data

city_df = pd.read_csv('raw_data/city_data.csv')
ride_df = pd.read_csv('raw_data/ride_data.csv')

```


```python
#city_df.head()
```


```python
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
#check for nulls
#print(city_df.count())
#print(ride_df.count())

#no nulls
```


```python
# merge city data and ride data for full view

merged_rides_df = pd.merge(city_df,ride_df, on='city')
#merged_rides_df.count()
#no nulls
merged_rides_df.head()
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>



# Bubble plot of ride sharing data


```python
# extract series that need to be plotted. 
# rides per city (x axis)
# avergae fare per city (y axis)
# drivers per city (bubble plot size)

# separate into city types
rural_rides_df = merged_rides_df[merged_rides_df['type'] == 'Rural']
suburb_rides_df = merged_rides_df[merged_rides_df['type'] == 'Suburban']
urban_rides_df = merged_rides_df[merged_rides_df['type'] == 'Urban']

# group data set by city and extract series for each type
r_grouped_rides_by_city_df = rural_rides_df.groupby('city')
r_rides_per_city = r_grouped_rides_by_city_df['ride_id'].count()
r_averagefare_per_city = r_grouped_rides_by_city_df['fare'].mean()
r_drivers_per_city = r_grouped_rides_by_city_df['driver_count'].sum()

s_grouped_rides_by_city_df = suburb_rides_df.groupby('city')
s_rides_per_city = s_grouped_rides_by_city_df['ride_id'].count()
s_averagefare_per_city = s_grouped_rides_by_city_df['fare'].mean()
s_drivers_per_city = s_grouped_rides_by_city_df['driver_count'].sum()

u_grouped_rides_by_city_df = urban_rides_df.groupby('city')
u_rides_per_city = u_grouped_rides_by_city_df['ride_id'].count()
u_averagefare_per_city = u_grouped_rides_by_city_df['fare'].mean()
u_drivers_per_city = u_grouped_rides_by_city_df['driver_count'].sum()


r_rides_per_city.head()
```




    city
    East Leslie       11
    East Stephen      10
    East Troybury      7
    Erikport           8
    Hernandezshire     9
    Name: ride_id, dtype: int64




```python
#using seaborn settings
sns.set()

fig, ax = plt.subplots(figsize=(10,10))
pyber_colors = ['gold','lightskyblue','lightcoral']
labels = ['Rural','Suburban','Urban']


ax.scatter(r_rides_per_city, r_averagefare_per_city, s= r_drivers_per_city, 
           marker="o", facecolors=pyber_colors[0], edgecolors="black", alpha=0.7, label=labels[0])
ax.scatter(s_rides_per_city, s_averagefare_per_city, s= s_drivers_per_city, 
           marker="o", facecolors=pyber_colors[1], edgecolors="black", alpha=0.7, label=labels[1])
ax.scatter(u_rides_per_city, u_averagefare_per_city, s= u_drivers_per_city, 
           marker="o", facecolors=pyber_colors[2], edgecolors="black", alpha=0.7, label=labels[2])

plt.title("Total rides vs Average fare", fontsize=14)
plt.xlabel("Total rides")
plt.ylabel("Average fare ($s)")
#plt.text(1, 1, "Circle size corelates to number of drivers", fontsize=12)
lgnd = plt.legend(loc='upper right', prop={'size': 12})
lgnd.legendHandles[0]._sizes = [50]
lgnd.legendHandles[1]._sizes = [50]
lgnd.legendHandles[2]._sizes = [50]
plt.show()
plt.clf()
plt.cla()
plt.close()
```


![png](output_11_0.png)



```python
#plot again after removing outliers to get a closer look at the majority cities

#remove max rides outlier
max_s_rides = s_rides_per_city.idxmax()
print("Dropping maximum rides outlier "+max_s_rides)
print(f"Rides : {str(s_rides_per_city[max_s_rides])} Average Fare : {str(round(s_averagefare_per_city[max_s_rides],0))}")
s_rides_per_city.drop(max_s_rides, inplace=True)
s_averagefare_per_city.drop(max_s_rides, inplace=True)
s_drivers_per_city.drop(max_s_rides, inplace=True)

#dropping rides less than five
print('--------------------')
min_r_rides = r_rides_per_city.idxmin()
print("Dropping minimum rides outlier "+min_r_rides)
#print(f"Rides : {str(s_rides_per_city[min_r_rides])} Average Fare : {str(round(r_averagefare_per_city[min_r_rides],0))}")
r_rides_per_city.drop(min_r_rides, inplace=True)
r_averagefare_per_city.drop(min_r_rides, inplace=True)
r_drivers_per_city.drop(min_r_rides, inplace=True)
```

    Dropping maximum rides outlier Port James
    Rides : 64 Average Fare : 32.0
    --------------------
    Dropping minimum rides outlier Manuelchester
    


```python


# redraw bubble plot
sns.set()

fig, ax = plt.subplots(figsize=(10,10))
pyber_colors = ['gold','lightskyblue','lightcoral']
labels = ['Rural','Suburban','Urban']

ax.scatter(r_rides_per_city, r_averagefare_per_city, s= r_drivers_per_city, 
           marker="o", facecolors=pyber_colors[0], edgecolors="black", alpha=0.7, label=labels[0])
ax.scatter(s_rides_per_city, s_averagefare_per_city, s= s_drivers_per_city, 
           marker="o", facecolors=pyber_colors[1], edgecolors="black", alpha=0.7, label=labels[1])
ax.scatter(u_rides_per_city, u_averagefare_per_city, s= u_drivers_per_city, 
           marker="o", facecolors=pyber_colors[2], edgecolors="black", alpha=0.7, label=labels[2])

plt.title("Pyber ride sharing data (2016)", fontsize=18)
plt.xlabel("Total rides")
plt.ylabel("Average fare ($s)")
lgnd = plt.legend(loc='upper right', prop={'size': 12})
lgnd.legendHandles[0]._sizes = [50]
lgnd.legendHandles[1]._sizes = [50]
lgnd.legendHandles[2]._sizes = [50]
plt.show()
plt.clf()
plt.cla()
plt.close()
```


![png](output_13_0.png)


# Total Fares by City Type


```python
#% of Total Fares by City Type

total_fare = ride_df['fare'].sum()

#print(total_fare)

group_by_city_type = merged_rides_df.groupby('type')

fares_by_city_type = group_by_city_type['fare'].sum()
#print(fares_by_city_type)

fares_by_city_type.plot.pie(labels=labels, colors=pyber_colors, autopct='%1.1f%%', explode=(0.1,0.1,0), startangle=10, fontsize=12, figsize=(6, 6))
plt.title("% Total Fares by City type", fontsize=18)
```




    Text(0.5,1,'% Total Fares by City type')




![png](output_15_1.png)



```python
#% of Total Rides by City Type

total_rides = ride_df['ride_id'].count()

#print(total_fare)

group_by_city_type = merged_rides_df.groupby('type')
fares_by_city_type = group_by_city_type['ride_id'].count()
#print(fares_by_city_type)

fares_by_city_type.plot.pie(labels=labels, colors=pyber_colors, explode=(0.1,0.1,0), autopct='%.2f', startangle=10, fontsize=12, figsize=(6, 6))
plt.title("% Total Rides by City type", fontsize=18)
```




    Text(0.5,1,'% Total Rides by City type')




![png](output_16_1.png)



```python
#% of Total Drivers by City Type

total_drivers = city_df['driver_count'].sum()

#print(total_fare)

group_by_city_type = city_df.groupby('type')
fares_by_city_type = group_by_city_type['driver_count'].sum()
#print(fares_by_city_type)

fares_by_city_type.plot.pie(labels=labels, colors=pyber_colors, explode=(0.1,0.1,0), autopct='%.2f', startangle=10, fontsize=12, figsize=(6, 6))
plt.title("% Total Drivers by City type", fontsize=18)
```




    Text(0.5,1,'% Total Drivers by City type')




![png](output_17_1.png)

