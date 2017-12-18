

```python
import pandas as pd
import csv
import matplotlib.pyplot as plt
```


```python
#Create the dataframes for each dataset
city_data = pd.read_csv("city_data.csv")
ride_data = pd.read_csv("ride_data.csv")
city_data.head()
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
#Data Cleaning: removing the Port James entry duplicates from the dataframes
city_data = city_data[city_data['city'] != "Port James"]
city_data.set_index("city", inplace=True)
city_data.sort_index(inplace=True)
ride_data = ride_data[ride_data["city"] != "Port James"]
ride_data.set_index("city", inplace=True)
ride_data.sort_index(inplace=True)
```


```python
#Merging the dataframes
pyber_df = pd.merge(city_data, ride_data, how="left", left_index=True, right_index=True)
pyber_df.head()
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
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>21</td>
      <td>Urban</td>
      <td>2016-04-25 08:50:08</td>
      <td>31.82</td>
      <td>7948246793429</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>21</td>
      <td>Urban</td>
      <td>2016-04-04 23:45:50</td>
      <td>14.25</td>
      <td>6431434271355</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>21</td>
      <td>Urban</td>
      <td>2016-01-25 06:02:25</td>
      <td>5.16</td>
      <td>2233026076010</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>21</td>
      <td>Urban</td>
      <td>2016-09-23 21:51:59</td>
      <td>17.67</td>
      <td>3829336915201</td>
    </tr>
    <tr>
      <th>Alvarezhaven</th>
      <td>21</td>
      <td>Urban</td>
      <td>2016-01-28 23:53:55</td>
      <td>9.87</td>
      <td>2747592323442</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculating Average Fare per city
avg_fare = pyber_df.groupby("city")["fare"].mean()
#Calculating Total Rides per city
total_rides = pyber_df.groupby("city")["ride_id"].count()
#Listing the Total Drivers count per city
total_drivers = city_data["driver_count"]
#Listing the city type for each city
city_type = city_data["type"]
```


```python
#Creating a Dataframe with avg_fare, total_rides, total_drivers, and type
df = pd.DataFrame({"Avg. Fare": avg_fare})
df["Total Rides"] = total_rides 
df["Total Drivers"] = total_drivers
df["City Type"] = city_type
df.head()
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
      <th>Avg. Fare</th>
      <th>Total Rides</th>
      <th>Total Drivers</th>
      <th>City Type</th>
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
      <td>31</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>26</td>
      <td>67</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.315556</td>
      <td>9</td>
      <td>16</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>22</td>
      <td>21</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>19</td>
      <td>49</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Creating the Bubble Plot for each city type
plt.subplot(facecolor='ivory')

plt.scatter(df[df["City Type"] == "Urban"]["Total Rides"], df[df["City Type"] == "Urban"]["Avg. Fare"], 
            s = df[df["City Type"] == "Urban"]["Total Drivers"] *2, alpha = .65, color = "lightcoral", label = "Urban")
plt.scatter(df[df["City Type"] == "Suburban"]["Total Rides"], df[df["City Type"] == "Suburban"]["Avg. Fare"], 
            s = df[df["City Type"] == "Suburban"]["Total Drivers"] *2, alpha = .65, color = "lightskyblue", label = "Suburban")
plt.scatter(df[df["City Type"] == "Rural"]["Total Rides"], df[df["City Type"] == "Rural"]["Avg. Fare"], 
            s = df[df["City Type"] == "Rural"]["Total Drivers"]*2, alpha = .65, color = "gold", label = "Rural")
# Create legends and set legend key size
lgnd = plt.legend()
lgnd.legendHandles[0]._sizes = [30]
lgnd.legendHandles[1]._sizes = [30]
lgnd.legendHandles[2]._sizes = [30]
# Create Lables and title
plt.xlabel("Total Number of Rides (Per City)")
plt.ylabel("Average Fare ($)")
plt.title("Pyber Ride Sharing data 2016")
#Changing the axis
plt.xlim(0,40)
plt.ylim(15,55)
# Create Grid
plt.grid()

```


```python
plt.show()
```


![png](output_7_0.png)



```python
#Create a list of Percentage of Total Fare by City Type
per_total_fare = []
total_fare_bytype = pyber_df.groupby("type")["fare"].sum()
total_sum = total_fare_bytype.sum()
per_total_fare.append(total_fare_bytype["Urban"]/total_sum)
per_total_fare.append(total_fare_bytype["Suburban"]/total_su)
per_total_fare.append(total_fare_bytype["Rural"]/total_sum)
```




    [0.63988664213240498, 0.29217702986421018, 0.067936328003384769]




```python
#Setting up Pie Chart
labels = ["Urban" , "Suburban", "Rural"]
colors = ["lightcoral", "lightskyblue", "gold"]
explode = (0.1, 0, 0)
#Create Pie Chart
plt.pie(per_total_fare, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=-130)
plt.axis("equal")
#Assign Title
plt.title("% of Total Fares by City Type")
```




    Text(0.5,1,'% of Total Fares by City Type')




```python
plt.show()
```


![png](output_10_0.png)



```python
#Create a list of Percentage of Total Rides by City Type
per_total_ride = []
total_ride_bytype = df.groupby("City Type")["Total Rides"].sum()
total_rsum = total_ride_bytype.sum()
per_total_ride.append(total_ride_bytype["Urban"]/total_rsum)
per_total_ride.append(total_ride_bytype["Suburban"]/total_rsum)
per_total_ride.append(total_ride_bytype["Rural"]/total_rsum)
```




    City Type
    Rural        125
    Suburban     593
    Urban       1625
    Name: Total Rides, dtype: int64




```python
#Setting up Pie Chart
labels = ["Urban" , "Suburban", "Rural"]
colors = ["lightcoral", "lightskyblue", "gold"]
explode = (0.1, 0, 0)
#Create Pie Chart
plt.pie(per_total_ride, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=-130)
plt.axis("equal")
#Assign Title
plt.title("% of Total Rides by City Type")

plt.show()
```


![png](output_12_0.png)

#Create a list of Percentage of Total Divers by City Type
per_total_drivers = []
total_drivers_bytype = df.groupby("City Type")["Total Drivers"].sum()
total_dsum = total_drivers_bytype.sum()
per_total_drivers.append(total_drivers_bytype["Urban"]/total_dsum)
per_total_drivers.append(total_drivers_bytype["Suburban"]/total_dsum)
per_total_drivers.append(total_drivers_bytype["Rural"]/total_dsum)

```python
#Setting up Pie Chart
labels = ["Urban" , "Suburban", "Rural"]
colors = ["lightcoral", "lightskyblue", "gold"]
explode = (0.1, 0, 0)
#Create Pie Chart
plt.pie(per_total_drivers, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=-130)
plt.axis("equal")
#Assign Title
plt.title("% of Total Drivers by City Type")

plt.show()
```


![png](output_14_0.png)

