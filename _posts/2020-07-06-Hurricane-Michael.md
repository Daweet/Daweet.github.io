# Hurricane Michael

>[Hurricane Michael](https://en.wikipedia.org/wiki/Hurricane_Michael) was a very powerful and destructive tropical cyclone that became the first Category 5 hurricane to strike the contiguous United States since Andrew in 1992. In addition, it was the third-most intense Atlantic hurricane to make landfall in the contiguous United States in terms of pressure, behind the 1935 Labor Day hurricane and Hurricane Camille in 1969. It was the first Category 5 hurricane on record to impact the Florida Panhandle, the fourth-strongest landfalling hurricane in the contiguous United States, in terms of wind speed, and was the most intense hurricane on record to strike the United States in the month of October.

In this post we will use the data for Hurricane Michael along with the json data for US map to trace out the trajectory of the hurricane as it traverses the contiguous United States. 
The [contiguous](https://en.wikipedia.org/wiki/Contiguous_United_States) United States or officially the conterminous United States consists of the 48 adjoining U.S. states (plus the District of Columbia) on the continent of North America. The terms exclude the non-contiguous states of Alaska and Hawaii, and all other off-shore insular areas, such as American Samoa, U.S. Virgin Islands, Northern Mariana Islands, Guam and Puerto Rico. 
Therefore using the geopandas library, we will turn the latitude and longitude columns into a geographical Point data type then make a geodataframe. On top of this we plot the path of Hurricane Michael onto the US map in the GeoJSON file.

Note

*    After loading the US_states(5m).json file as a geodataframe, we will use the following code to create a geodataframe that only contains the contiguous United States (48 states):
`python
map48 = map_df.loc[map_df['NAME'].isin(['Alaska', 'Hawaii', 'Puerto Rico']) == False]`

* The longitude column data should be turned into negative values(data source listed longitude direction instead of positive/negative). Use the following code to make the data correct:
`python
df['Long'] = 0 - df['Long']`



```python
import pandas as pd
import geopandas as gpd #used for transforming geolocation data
import matplotlib.pyplot as plt

from datetime import datetime  #to convert data to datetime that does not fall within the pandas.to_datetime function timeframe
from shapely.geometry import Point  #transform latitude/longitude to geo-coordinate data
from geopandas.tools import geocode #get the latitude/longitude for a given address
from geopandas.tools import reverse_geocode  #get the address for a location using latitude/longitude

%matplotlib inline
```

### Geocoding and Reverse Geocoding

Geocoding is taking an address for a location and returning its latitudinal and longitudinal coordinates. Reverse geocoding would then be the opposite - taking the latitudinal and longitudinal coordinates for a location and returning the physical address.


```python
#load hurricane data collected 
hurricane_df = pd.read_csv("datasets/hurricaneMichael.csv")
hurricane_df.head()
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
      <th>AdvisoryNumber</th>
      <th>Date</th>
      <th>Lat</th>
      <th>Long</th>
      <th>Wind</th>
      <th>Pres</th>
      <th>Movement</th>
      <th>Type</th>
      <th>Name</th>
      <th>Received</th>
      <th>Forecaster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>10/06/2018 17:00</td>
      <td>18.0</td>
      <td>86.6</td>
      <td>30</td>
      <td>1006</td>
      <td>NW at 6 MPH (325 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 16:50</td>
      <td>Beven</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1A</td>
      <td>10/06/2018 20:00</td>
      <td>18.3</td>
      <td>86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 6 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 19:32</td>
      <td>Avila</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>10/06/2018 23:00</td>
      <td>18.8</td>
      <td>86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 7 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 22:38</td>
      <td>Avila</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2A</td>
      <td>10/07/2018 02:00</td>
      <td>18.4</td>
      <td>87.1</td>
      <td>35</td>
      <td>1004</td>
      <td>NW at 5 MPH (320 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/07/2018 01:38</td>
      <td>Berg</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>10/07/2018 05:00</td>
      <td>18.6</td>
      <td>86.9</td>
      <td>35</td>
      <td>1004</td>
      <td>NNW at 3 MPH (340 deg)</td>
      <td>Tropical Depression</td>
      <td>FOURTEEN</td>
      <td>10/07/2018 04:53</td>
      <td>Berg</td>
    </tr>
  </tbody>
</table>
</div>




```python
hurricane_df['Long'] = 0 - hurricane_df['Long']
```


```python
#data type of each column
hurricane_df.dtypes
```




    AdvisoryNumber     object
    Date               object
    Lat               float64
    Long              float64
    Wind                int64
    Pres                int64
    Movement           object
    Type               object
    Name               object
    Received           object
    Forecaster         object
    dtype: object




```python
len(hurricane_df)
```




    45




```python
#see columns with null values
hurricane_df.count()
```




    AdvisoryNumber    45
    Date              45
    Lat               45
    Long              45
    Wind              45
    Pres              45
    Movement          45
    Type              45
    Name              45
    Received          45
    Forecaster        45
    dtype: int64




```python
#make a new column to hold the longitude & latitude as a list
hurricane_df['coordinates'] = list(hurricane_df[['Long', 'Lat']].values)
```


```python
#see new coordinates column
hurricane_df.head()
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
      <th>AdvisoryNumber</th>
      <th>Date</th>
      <th>Lat</th>
      <th>Long</th>
      <th>Wind</th>
      <th>Pres</th>
      <th>Movement</th>
      <th>Type</th>
      <th>Name</th>
      <th>Received</th>
      <th>Forecaster</th>
      <th>coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>10/06/2018 17:00</td>
      <td>18.0</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1006</td>
      <td>NW at 6 MPH (325 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 16:50</td>
      <td>Beven</td>
      <td>[-86.6, 18.0]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1A</td>
      <td>10/06/2018 20:00</td>
      <td>18.3</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 6 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 19:32</td>
      <td>Avila</td>
      <td>[-86.6, 18.3]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>10/06/2018 23:00</td>
      <td>18.8</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 7 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 22:38</td>
      <td>Avila</td>
      <td>[-86.6, 18.8]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2A</td>
      <td>10/07/2018 02:00</td>
      <td>18.4</td>
      <td>-87.1</td>
      <td>35</td>
      <td>1004</td>
      <td>NW at 5 MPH (320 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/07/2018 01:38</td>
      <td>Berg</td>
      <td>[-87.1, 18.4]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>10/07/2018 05:00</td>
      <td>18.6</td>
      <td>-86.9</td>
      <td>35</td>
      <td>1004</td>
      <td>NNW at 3 MPH (340 deg)</td>
      <td>Tropical Depression</td>
      <td>FOURTEEN</td>
      <td>10/07/2018 04:53</td>
      <td>Berg</td>
      <td>[-86.9, 18.6]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list values in coordinates column is classified as object type
hurricane_df['coordinates'].dtypes
```




    dtype('O')




```python
#convert the coordinates to a geolocation type
hurricane_df['coordinates'] = hurricane_df['coordinates'].apply(Point)
```


```python
#coordinates column now has POINT next to each coordinate pair value
hurricane_df.head()
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
      <th>AdvisoryNumber</th>
      <th>Date</th>
      <th>Lat</th>
      <th>Long</th>
      <th>Wind</th>
      <th>Pres</th>
      <th>Movement</th>
      <th>Type</th>
      <th>Name</th>
      <th>Received</th>
      <th>Forecaster</th>
      <th>coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>10/06/2018 17:00</td>
      <td>18.0</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1006</td>
      <td>NW at 6 MPH (325 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 16:50</td>
      <td>Beven</td>
      <td>POINT (-86.59999999999999 18)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1A</td>
      <td>10/06/2018 20:00</td>
      <td>18.3</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 6 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 19:32</td>
      <td>Avila</td>
      <td>POINT (-86.59999999999999 18.3)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>10/06/2018 23:00</td>
      <td>18.8</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 7 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 22:38</td>
      <td>Avila</td>
      <td>POINT (-86.59999999999999 18.8)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2A</td>
      <td>10/07/2018 02:00</td>
      <td>18.4</td>
      <td>-87.1</td>
      <td>35</td>
      <td>1004</td>
      <td>NW at 5 MPH (320 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/07/2018 01:38</td>
      <td>Berg</td>
      <td>POINT (-87.09999999999999 18.4)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>10/07/2018 05:00</td>
      <td>18.6</td>
      <td>-86.9</td>
      <td>35</td>
      <td>1004</td>
      <td>NNW at 3 MPH (340 deg)</td>
      <td>Tropical Depression</td>
      <td>FOURTEEN</td>
      <td>10/07/2018 04:53</td>
      <td>Berg</td>
      <td>POINT (-86.90000000000001 18.6)</td>
    </tr>
  </tbody>
</table>
</div>




```python
#coordinates column with geolocation data is just a regular pandas Series type
type(hurricane_df['coordinates'])
```




    pandas.core.series.Series




```python
#create a geolocation dataframe type using the coordinates column as the geolocation data
geo_hurricane = gpd.GeoDataFrame(hurricane_df, geometry='coordinates')
```


```python
#geo-dataframe looks the same as regular dataframe
geo_hurricane.head()
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
      <th>AdvisoryNumber</th>
      <th>Date</th>
      <th>Lat</th>
      <th>Long</th>
      <th>Wind</th>
      <th>Pres</th>
      <th>Movement</th>
      <th>Type</th>
      <th>Name</th>
      <th>Received</th>
      <th>Forecaster</th>
      <th>coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>10/06/2018 17:00</td>
      <td>18.0</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1006</td>
      <td>NW at 6 MPH (325 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 16:50</td>
      <td>Beven</td>
      <td>POINT (-86.60000 18.00000)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1A</td>
      <td>10/06/2018 20:00</td>
      <td>18.3</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 6 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 19:32</td>
      <td>Avila</td>
      <td>POINT (-86.60000 18.30000)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>10/06/2018 23:00</td>
      <td>18.8</td>
      <td>-86.6</td>
      <td>30</td>
      <td>1004</td>
      <td>N at 7 MPH (360 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/06/2018 22:38</td>
      <td>Avila</td>
      <td>POINT (-86.60000 18.80000)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2A</td>
      <td>10/07/2018 02:00</td>
      <td>18.4</td>
      <td>-87.1</td>
      <td>35</td>
      <td>1004</td>
      <td>NW at 5 MPH (320 deg)</td>
      <td>Potential Tropical Cyclone</td>
      <td>Fourteen</td>
      <td>10/07/2018 01:38</td>
      <td>Berg</td>
      <td>POINT (-87.10000 18.40000)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>10/07/2018 05:00</td>
      <td>18.6</td>
      <td>-86.9</td>
      <td>35</td>
      <td>1004</td>
      <td>NNW at 3 MPH (340 deg)</td>
      <td>Tropical Depression</td>
      <td>FOURTEEN</td>
      <td>10/07/2018 04:53</td>
      <td>Berg</td>
      <td>POINT (-86.90000 18.60000)</td>
    </tr>
  </tbody>
</table>
</div>




```python
#verify coordinates column is geolocation data type
type(geo_hurricane['coordinates'])
```




    geopandas.geoseries.GeoSeries




```python
#import file that contains a US map shape polygons
#will use to plot the coordinates of meteorite landings
filepath = "datasets/US_states(5m).json"

#data contains polygon shape coordinates for different map body types (states, etc.)
map_df = gpd.read_file(filepath)
map48 = map_df.loc[map_df['NAME'].isin(['Alaska', 'Hawaii', 'Puerto Rico']) == False]
map48.head()
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
      <th>GEO_ID</th>
      <th>STATE</th>
      <th>NAME</th>
      <th>LSAD</th>
      <th>CENSUSAREA</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0400000US01</td>
      <td>01</td>
      <td>Alabama</td>
      <td></td>
      <td>50645.326</td>
      <td>MULTIPOLYGON (((-88.12466 30.28364, -88.08681 ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0400000US04</td>
      <td>04</td>
      <td>Arizona</td>
      <td></td>
      <td>113594.084</td>
      <td>POLYGON ((-112.53859 37.00067, -112.53454 37.0...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0400000US05</td>
      <td>05</td>
      <td>Arkansas</td>
      <td></td>
      <td>52035.477</td>
      <td>POLYGON ((-94.04296 33.01922, -94.04304 33.079...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0400000US06</td>
      <td>06</td>
      <td>California</td>
      <td></td>
      <td>155779.220</td>
      <td>MULTIPOLYGON (((-122.42144 37.86997, -122.4213...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0400000US08</td>
      <td>08</td>
      <td>Colorado</td>
      <td></td>
      <td>103641.888</td>
      <td>POLYGON ((-106.19055 40.99761, -106.06118 40.9...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#map graph
map48.plot(cmap='OrRd')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fd37a3a4f50>




![png](output_18_1.png)



```python
#plot the coordinates (no map)
geo_hurricane.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fd37a8a8110>




![png](output_19_1.png)



```python
#plot coordinates on top of map graph

#this is to set the size of the borders
fig, ax = plt.subplots(1, figsize=(10,15))

#this is the map , cmap='OrRd'
basemap = map48.plot(ax=ax,  cmap='OrRd')

#plot coordinates on top of map graph
geo_hurricane.plot(ax=basemap, color='black', marker="o", markersize=50)

#take off axis numbers
ax.axis('off')

#put title on map
ax.set_ylim([17, 58])
ax.set_title("Hurrican Michael Trajectory", fontsize=25, fontweight=3)
```




    Text(0.5, 1, 'Hurrican Michael Trajectory')




![png](output_20_1.png)


### We could zoom-in and see the trajectory 


```python
#plot coordinates on top of map graph

#this is to set the size of the borders
fig, ax = plt.subplots(1, figsize=(10,15))

#this is the map , cmap='OrRd'
basemap = map48.plot(ax=ax,  cmap='OrRd')

#plot coordinates on top of map graph
geo_hurricane.plot(ax=basemap, color='black', marker=">", markersize=50)

#take off axis numbers
ax.axis('off')

#put title on map
ax.set_xlim([-90, -75])
ax.set_ylim([17.5, 37.5])
ax.set_title("Hurrican Michael Trajectory", fontsize=25, fontweight=3)
```




    Text(0.5, 1, 'Hurrican Michael Trajectory')




![png](output_22_1.png)



```python

```
