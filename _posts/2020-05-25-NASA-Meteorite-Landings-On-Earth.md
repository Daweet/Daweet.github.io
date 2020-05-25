---
layout: post
title: "NASA Earth Meteorite Landings"
category: Data Science, Physics
tags: [meteor, json, NASA, Geopandas]
date: 2020-05-25
---


This post shows you how to merge geographic map (which we get from from json file format) and data points (in this case meteorite landings) into a single map and visualize the data.

We will use the URL from NASA's Open Data Portal API from [NASA]("https://data.nasa.gov/resource/y77d-th95.json"), it is a comprehensive data set from The Meteoritical Society. The dataset contains information on all of the known meteorite landings. For the sake of this tutorial we will request data and import it as JSON. Our target is to extract the following features from that data to create a dataframe and then map the points to visualize the locations of the landings. The columns we are interested are listed below:

    -ID
    -Year
    -Fall
    -Name
    -Name Type
    -Mass
    -Latitude
    -Longitude
    
As usual let us import all the necessary libraries that we are going to use.


```python
import pandas as pd
import geopandas as gpd #used for transforming geolocation data
import matplotlib.pyplot as plt

from datetime import datetime  #to convert data to datetime that does not fall 
#within the pandas.to_datetime function timeframe
from shapely.geometry import Point  #transform latitude/longitude to geo-coordinate data
from geopandas.tools import geocode #get the latitude/longitude for a given address
from geopandas.tools import reverse_geocode  #get the address for a location using latitude/longitude

import requests  #similar to urllib, this library allows a computer to ping a website
import json      #library to handle JSON formatted data
%matplotlib inline
```

The url we are accessing needs no authentication. When accesing APIs it is better to check if we need authentication keys or not. Besides that, it is also worth cheking the sharing protocol for each websites. Next we use requests library to request the url.


```python

#URL to NASA meteorite API
url = r"https://data.nasa.gov/resource/y77d-th95.json"

```


```python
#the get function checks to make sure that the website/server is responding back
#200 means that we're good
#https://www.restapitutorial.com/httpstatuscodes.html
resp = requests.get(url)
resp
```




    <Response [200]>



We can check the response status code using the above code line, for further information you can check the [website]('https://www.restapitutorial.com/httpstatuscodes.html'). Once we get the `<Response [200]>` we can go ahead and check if we really are accessing the site by printing it out. As it can be a large output, it is advisable to limit the content if possible. 


```python
#send a request to the website to return back text data from the API
#returns data as JSON string
str_data = resp.text
str_data[:1500]
```




    '[{"name":"Aachen","id":"1","nametype":"Valid","recclass":"L5","mass":"21","fall":"Fell","year":"1880-01-01T00:00:00.000","reclat":"50.775000","reclong":"6.083330","geolocation":{"type":"Point","coordinates":[6.08333,50.775]}}\n,{"name":"Aarhus","id":"2","nametype":"Valid","recclass":"H6","mass":"720","fall":"Fell","year":"1951-01-01T00:00:00.000","reclat":"56.183330","reclong":"10.233330","geolocation":{"type":"Point","coordinates":[10.23333,56.18333]}}\n,{"name":"Abee","id":"6","nametype":"Valid","recclass":"EH4","mass":"107000","fall":"Fell","year":"1952-01-01T00:00:00.000","reclat":"54.216670","reclong":"-113.000000","geolocation":{"type":"Point","coordinates":[-113,54.21667]}}\n,{"name":"Acapulco","id":"10","nametype":"Valid","recclass":"Acapulcoite","mass":"1914","fall":"Fell","year":"1976-01-01T00:00:00.000","reclat":"16.883330","reclong":"-99.900000","geolocation":{"type":"Point","coordinates":[-99.9,16.88333]}}\n,{"name":"Achiras","id":"370","nametype":"Valid","recclass":"L6","mass":"780","fall":"Fell","year":"1902-01-01T00:00:00.000","reclat":"-33.166670","reclong":"-64.950000","geolocation":{"type":"Point","coordinates":[-64.95,-33.16667]}}\n,{"name":"Adhi Kot","id":"379","nametype":"Valid","recclass":"EH4","mass":"4239","fall":"Fell","year":"1919-01-01T00:00:00.000","reclat":"32.100000","reclong":"71.800000","geolocation":{"type":"Point","coordinates":[71.8,32.1]}}\n,{"name":"Adzhi-Bogdo (stone)","id":"390","nametype":"Valid","recclass":"LL3-6","mass":"910","fall":"Fell"'



We need to decode the json file and that can be achieved as follows


```python
#loads function reversed dictionary order
#dictionary objects are unordered in general
NASAdata = json.loads(str_data)
#NASAdata
```

It is also better to verify that JSON object is list and check its length,  this is done by

```Python
type(NASAdata)
len(NASAdata)```

To help us navigate through the tree, we keep asking the types until we reach the end of the hierarchical line.


```python
#first level keys in JSON object
NASAdata[0]
```




    {'name': 'Aachen',
     'id': '1',
     'nametype': 'Valid',
     'recclass': 'L5',
     'mass': '21',
     'fall': 'Fell',
     'year': '1880-01-01T00:00:00.000',
     'reclat': '50.775000',
     'reclong': '6.083330',
     'geolocation': {'type': 'Point', 'coordinates': [6.08333, 50.775]}}




```python

#dict['key']['key'][index]
#dictionary name, dictionary key, dictionary key, then list index
NASAdata[0]['geolocation']

```




    {'type': 'Point', 'coordinates': [6.08333, 50.775]}




```python
type(NASAdata[0]['geolocation'])
```




    dict




```python
NASAdata[0]['geolocation'].keys()
```




    dict_keys(['type', 'coordinates'])




```python

#dict['key']['key'][index]
#dictionary name, dictionary key, dictionary key, then list index
NASAdata[0]['geolocation']['type']

```




    'Point'




```python

type(NASAdata[0]['geolocation']['type'])

```




    str




```python

#dict['key']['key'][index]
#dictionary name, dictionary key, dictionary key, then list index
NASAdata[0]['geolocation']['coordinates']

```




    [6.08333, 50.775]




```python
type(NASAdata[0]['geolocation']['coordinates'])
```




    list




```python
len(NASAdata[0]['geolocation']['coordinates'])
```




    2




```python
NASAdata[0]['geolocation']['coordinates'][0]
```




    6.08333



Let us now create an empyt list where we will put the retrivied information. We use the notation **namels**, where name is the name of the column and *ls* indicate that we are creating new list.


```python
idmls = []
yearls = []
fallls = []
namels = [] 
nametypels = []
massls = []
latitudels = []
longitudels = []
```


```python
#extract the values for the meteore dataset

for meteor in NASAdata:
      
    #get the value of each key and if the key doesn't exist, set a variable to be None
    try: idm = meteor['id']
    except: idm = None
            
    try: year = meteor['year']
    except: year = None
            
    try: fall = meteor['fall']
    except: fall = None
        
    try: name = meteor['name']
    except: name = None
            
    try: nametype = meteor['nametype']
    except: nametype = None
            
    try: mass = meteor['mass']
    except: mass = None
            
    try: latitude =meteor['geolocation']['coordinates'][1]
    except: latitude = None
            
    try: longitude = meteor['geolocation']['coordinates'][0]
    except: longitude = None
            
    
    idmls.append(idm)
    yearls.append(year)
    fallls.append(fall)
    namels.append(name)  
    nametypels.append(nametype)
    massls.append(mass) 
    latitudels.append(latitude)
    longitudels.append(longitude) 
```


```python
#check to see first 5 items of a few lists
print(idmls[:5])
print(yearls[:5])
print(fallls[:5])
print(namels[:5])
print(nametypels[:5])
print(massls[:5])
```

    ['1', '2', '6', '10', '370']
    ['1880-01-01T00:00:00.000', '1951-01-01T00:00:00.000', '1952-01-01T00:00:00.000', '1976-01-01T00:00:00.000', '1902-01-01T00:00:00.000']
    ['Fell', 'Fell', 'Fell', 'Fell', 'Fell']
    ['Aachen', 'Aarhus', 'Abee', 'Acapulco', 'Achiras']
    ['Valid', 'Valid', 'Valid', 'Valid', 'Valid']
    ['21', '720', '107000', '1914', '780']



```python
#check number of items in each list
print(len(idmls))
print(len(yearls))
print(len(fallls))
print(len(namels))
print(len(nametypels))
print(len(massls))
print(len(latitudels))
print(len(longitudels))

```

    1000
    1000
    1000
    1000
    1000
    1000
    1000
    1000



```python
#match indices of each list and zip into one list
meteorList = list(zip(idmls, yearls, fallls, namels, 
                    nametypels, massls, latitudels, longitudels))

#names for columns in data frame
colnames = ['id', 'year', 'fall','name', 'nametype', 'mass', 'latitude', 'longitude']
```


```python
#create data frame with column names in it
df = pd.DataFrame(meteorList, columns=colnames)

df.head(10)
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1880-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Aachen</td>
      <td>Valid</td>
      <td>21</td>
      <td>50.77500</td>
      <td>6.08333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1951-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Aarhus</td>
      <td>Valid</td>
      <td>720</td>
      <td>56.18333</td>
      <td>10.23333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1952-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Abee</td>
      <td>Valid</td>
      <td>107000</td>
      <td>54.21667</td>
      <td>-113.00000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1976-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Acapulco</td>
      <td>Valid</td>
      <td>1914</td>
      <td>16.88333</td>
      <td>-99.90000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>370</td>
      <td>1902-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Achiras</td>
      <td>Valid</td>
      <td>780</td>
      <td>-33.16667</td>
      <td>-64.95000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>379</td>
      <td>1919-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Adhi Kot</td>
      <td>Valid</td>
      <td>4239</td>
      <td>32.10000</td>
      <td>71.80000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>390</td>
      <td>1949-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Adzhi-Bogdo (stone)</td>
      <td>Valid</td>
      <td>910</td>
      <td>44.83333</td>
      <td>95.16667</td>
    </tr>
    <tr>
      <th>7</th>
      <td>392</td>
      <td>1814-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Agen</td>
      <td>Valid</td>
      <td>30000</td>
      <td>44.21667</td>
      <td>0.61667</td>
    </tr>
    <tr>
      <th>8</th>
      <td>398</td>
      <td>1930-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Aguada</td>
      <td>Valid</td>
      <td>1620</td>
      <td>-31.60000</td>
      <td>-65.23333</td>
    </tr>
    <tr>
      <th>9</th>
      <td>417</td>
      <td>1920-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Aguila Blanca</td>
      <td>Valid</td>
      <td>1440</td>
      <td>-30.86667</td>
      <td>-64.55000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail(10)
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>990</th>
      <td>23984</td>
      <td>1986-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tianzhang</td>
      <td>Valid</td>
      <td>2232</td>
      <td>32.94667</td>
      <td>118.99000</td>
    </tr>
    <tr>
      <th>991</th>
      <td>23989</td>
      <td>1878-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tieschitz</td>
      <td>Valid</td>
      <td>28000</td>
      <td>49.60000</td>
      <td>17.11667</td>
    </tr>
    <tr>
      <th>992</th>
      <td>23998</td>
      <td>1927-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tilden</td>
      <td>Valid</td>
      <td>74800</td>
      <td>38.20000</td>
      <td>-89.68333</td>
    </tr>
    <tr>
      <th>993</th>
      <td>23999</td>
      <td>1970-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tillaberi</td>
      <td>Valid</td>
      <td>3000</td>
      <td>14.25000</td>
      <td>1.53333</td>
    </tr>
    <tr>
      <th>994</th>
      <td>24004</td>
      <td>1807-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Timochin</td>
      <td>Valid</td>
      <td>65500</td>
      <td>54.50000</td>
      <td>35.20000</td>
    </tr>
    <tr>
      <th>995</th>
      <td>24009</td>
      <td>1934-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tirupati</td>
      <td>Valid</td>
      <td>230</td>
      <td>13.63333</td>
      <td>79.41667</td>
    </tr>
    <tr>
      <th>996</th>
      <td>54823</td>
      <td>2011-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tissint</td>
      <td>Valid</td>
      <td>7000</td>
      <td>29.48195</td>
      <td>-7.61123</td>
    </tr>
    <tr>
      <th>997</th>
      <td>24011</td>
      <td>1869-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tjabe</td>
      <td>Valid</td>
      <td>20000</td>
      <td>-7.08333</td>
      <td>111.53333</td>
    </tr>
    <tr>
      <th>998</th>
      <td>24012</td>
      <td>1922-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tjerebon</td>
      <td>Valid</td>
      <td>16500</td>
      <td>-6.66667</td>
      <td>106.58333</td>
    </tr>
    <tr>
      <th>999</th>
      <td>24019</td>
      <td>1905-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Tomakovka</td>
      <td>Valid</td>
      <td>600</td>
      <td>47.85000</td>
      <td>34.76667</td>
    </tr>
  </tbody>
</table>
</div>



# Geolocation Data
Geospatial information is data that is referenced by spatial or geographic coordinates. The data that we will be working with in this lesson is vector data - features that are represented by points, lines, and polygons. 
Points are defined by a pair of (x,y) coordinates. They usually represent locations, place names, and other objects on the ground.
Lines are the connection between two points. They can have properties such as length, direction, flow, etc.
Polygons are a series of lines connected together to form a shape. They can have properties such as area, perimeters, and centroids.
In this notebook, you will need to install the geopandas and geoPy libraries. Also, download this text file to use in the example code below.

## Geocoding and Reverse Geocoding
Geocoding is taking an address for a location and returning its latitudinal and longitudinal coordinates. Reverse geocoding would then be the opposite - taking the latitudinal and longitudinal coordinates for a location and returning the physical address.


```python
#take an address and return coordinates
#returned variable is a geo-dataframe with 2 columns, geometry (the geographical shape) and the full physical address
ex1_geo = geocode("1830 Metzerott rd, Adelphi, MD", provider='nominatim')
ex1_geo
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
      <th>geometry</th>
      <th>address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>POINT (-76.96662 39.00261)</td>
      <td>Metzerott Road, Avenel, Buck Lodge, Adelphi, P...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#structure of full address
#each API structures addresses differently
ex1_geo['address'].iloc[0]
```




    "Metzerott Road, Avenel, Buck Lodge, Adelphi, Prince George's County, Maryland, 20740, United States of America"




```python
#use latitude and longitude to get physical address
#pass through using Point geometry
#also returns geo-dataframe with geometry and full physical address
ex2_geo = reverse_geocode([Point(-77.15879730243169, 39.0985195)], provider='nominatim')
ex2_geo
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
      <th>geometry</th>
      <th>address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>POINT (-77.15880 39.09852)</td>
      <td>Montgomery College, 51, Mannakee Street, West ...</td>
    </tr>
  </tbody>
</table>
</div>



### Geocode a Dataframe column

We need to download the continents.json GeoJSON file for the world map that will be charted.


```python
meteor_df = df
meteor_df.head()
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1880-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Aachen</td>
      <td>Valid</td>
      <td>21</td>
      <td>50.77500</td>
      <td>6.08333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1951-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Aarhus</td>
      <td>Valid</td>
      <td>720</td>
      <td>56.18333</td>
      <td>10.23333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1952-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Abee</td>
      <td>Valid</td>
      <td>107000</td>
      <td>54.21667</td>
      <td>-113.00000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1976-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Acapulco</td>
      <td>Valid</td>
      <td>1914</td>
      <td>16.88333</td>
      <td>-99.90000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>370</td>
      <td>1902-01-01T00:00:00.000</td>
      <td>Fell</td>
      <td>Achiras</td>
      <td>Valid</td>
      <td>780</td>
      <td>-33.16667</td>
      <td>-64.95000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#data type of each column
meteor_df.dtypes
```




    id            object
    year          object
    fall          object
    name          object
    nametype      object
    mass          object
    latitude     float64
    longitude    float64
    dtype: object




```python
#only dataframe with non-null year column values
meteor_df = meteor_df.loc[meteor_df['year'].notnull()]

#change year column into a string
#need to use string type for getYear function below
meteor_df['year'] = meteor_df['year'].astype(str)
```

    
      



```python
#function to split apart the date from the timestamp
def getYear(col):
    #get YYYY-MM-DD value
    date = col.split("T")[0]
    
    #extract year from date
    dt = datetime.strptime(date, '%Y-%m-%d')
    return dt.year
```


```python
#replace the year timestamp data with only the year (using the getYear function)
meteor_df['year'] = meteor_df['year'].apply(getYear)
meteor_df.head()
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1880</td>
      <td>Fell</td>
      <td>Aachen</td>
      <td>Valid</td>
      <td>21</td>
      <td>50.77500</td>
      <td>6.08333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1951</td>
      <td>Fell</td>
      <td>Aarhus</td>
      <td>Valid</td>
      <td>720</td>
      <td>56.18333</td>
      <td>10.23333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1952</td>
      <td>Fell</td>
      <td>Abee</td>
      <td>Valid</td>
      <td>107000</td>
      <td>54.21667</td>
      <td>-113.00000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1976</td>
      <td>Fell</td>
      <td>Acapulco</td>
      <td>Valid</td>
      <td>1914</td>
      <td>16.88333</td>
      <td>-99.90000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>370</td>
      <td>1902</td>
      <td>Fell</td>
      <td>Achiras</td>
      <td>Valid</td>
      <td>780</td>
      <td>-33.16667</td>
      <td>-64.95000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#see columns with null values
meteor_df.count()
```




    id           999
    year         999
    fall         999
    name         999
    nametype     999
    mass         971
    latitude     987
    longitude    987
    dtype: int64




```python
#only include rows with non-null latitudes (which means longitude is also not null) and non-null mass
meteor_df = meteor_df.loc[(meteor_df['latitude'].notnull()) & meteor_df['mass'].notnull()]
meteor_df.count()
```




    id           959
    year         959
    fall         959
    name         959
    nametype     959
    mass         959
    latitude     959
    longitude    959
    dtype: int64




```python
#make a new column to hold the longitude & latitude as a list
meteor_df['coordinates'] = list(meteor_df[['longitude', 'latitude']].values)
```


```python
#see new coordinates column
meteor_df.head()
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1880</td>
      <td>Fell</td>
      <td>Aachen</td>
      <td>Valid</td>
      <td>21</td>
      <td>50.77500</td>
      <td>6.08333</td>
      <td>[6.08333, 50.775]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1951</td>
      <td>Fell</td>
      <td>Aarhus</td>
      <td>Valid</td>
      <td>720</td>
      <td>56.18333</td>
      <td>10.23333</td>
      <td>[10.23333, 56.18333]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1952</td>
      <td>Fell</td>
      <td>Abee</td>
      <td>Valid</td>
      <td>107000</td>
      <td>54.21667</td>
      <td>-113.00000</td>
      <td>[-113.0, 54.21667]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1976</td>
      <td>Fell</td>
      <td>Acapulco</td>
      <td>Valid</td>
      <td>1914</td>
      <td>16.88333</td>
      <td>-99.90000</td>
      <td>[-99.9, 16.88333]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>370</td>
      <td>1902</td>
      <td>Fell</td>
      <td>Achiras</td>
      <td>Valid</td>
      <td>780</td>
      <td>-33.16667</td>
      <td>-64.95000</td>
      <td>[-64.95, -33.16667]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list values in coordinates column is classified as object type
meteor_df['coordinates'].dtypes
```




    dtype('O')




```python
#convert the coordinates to a geolocation type
meteor_df['coordinates'] = meteor_df['coordinates'].apply(Point)
```


```python
#coordinates column now has POINT next to each coordinate pair value
meteor_df.head()
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1880</td>
      <td>Fell</td>
      <td>Aachen</td>
      <td>Valid</td>
      <td>21</td>
      <td>50.77500</td>
      <td>6.08333</td>
      <td>POINT (6.08333 50.775)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1951</td>
      <td>Fell</td>
      <td>Aarhus</td>
      <td>Valid</td>
      <td>720</td>
      <td>56.18333</td>
      <td>10.23333</td>
      <td>POINT (10.23333 56.18333)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1952</td>
      <td>Fell</td>
      <td>Abee</td>
      <td>Valid</td>
      <td>107000</td>
      <td>54.21667</td>
      <td>-113.00000</td>
      <td>POINT (-113 54.21667)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1976</td>
      <td>Fell</td>
      <td>Acapulco</td>
      <td>Valid</td>
      <td>1914</td>
      <td>16.88333</td>
      <td>-99.90000</td>
      <td>POINT (-99.90000000000001 16.88333)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>370</td>
      <td>1902</td>
      <td>Fell</td>
      <td>Achiras</td>
      <td>Valid</td>
      <td>780</td>
      <td>-33.16667</td>
      <td>-64.95000</td>
      <td>POINT (-64.95 -33.16667)</td>
    </tr>
  </tbody>
</table>
</div>




```python
#coordinates column with geolocation data is just a regular pandas Series type
type(meteor_df['coordinates'])
```




    pandas.core.series.Series




```python
#create a geolocation dataframe type using the coordinates column as the geolocation data
geo_meteor = gpd.GeoDataFrame(meteor_df, geometry='coordinates')
```


```python
#geo-dataframe looks the same as regular dataframe
geo_meteor.head()
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
      <th>id</th>
      <th>year</th>
      <th>fall</th>
      <th>name</th>
      <th>nametype</th>
      <th>mass</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>coordinates</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1880</td>
      <td>Fell</td>
      <td>Aachen</td>
      <td>Valid</td>
      <td>21</td>
      <td>50.77500</td>
      <td>6.08333</td>
      <td>POINT (6.08333 50.77500)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1951</td>
      <td>Fell</td>
      <td>Aarhus</td>
      <td>Valid</td>
      <td>720</td>
      <td>56.18333</td>
      <td>10.23333</td>
      <td>POINT (10.23333 56.18333)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1952</td>
      <td>Fell</td>
      <td>Abee</td>
      <td>Valid</td>
      <td>107000</td>
      <td>54.21667</td>
      <td>-113.00000</td>
      <td>POINT (-113.00000 54.21667)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1976</td>
      <td>Fell</td>
      <td>Acapulco</td>
      <td>Valid</td>
      <td>1914</td>
      <td>16.88333</td>
      <td>-99.90000</td>
      <td>POINT (-99.90000 16.88333)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>370</td>
      <td>1902</td>
      <td>Fell</td>
      <td>Achiras</td>
      <td>Valid</td>
      <td>780</td>
      <td>-33.16667</td>
      <td>-64.95000</td>
      <td>POINT (-64.95000 -33.16667)</td>
    </tr>
  </tbody>
</table>
</div>




```python
#verify coordinates column is geolocation data type
type(geo_meteor['coordinates'])
```




    geopandas.geoseries.GeoSeries



I already have the json data file with coordinates for the continents, but I presume it is not a difficult task to get the file online.


```python
#import file that contains a world map shape polygons
#will use to plot the coordinates of meteorite landings
filepath = "datasets/continents.json"

#data contains polygon shape coordinates for different map body types (continents, etc.)
map_df = gpd.read_file(filepath)
map_df.head()
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
      <th>CONTINENT</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Asia</td>
      <td>MULTIPOLYGON (((93.27554 80.26361, 93.14804 80...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>North America</td>
      <td>MULTIPOLYGON (((-25.28167 71.39166, -25.62389 ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Europe</td>
      <td>MULTIPOLYGON (((58.06138 81.68776, 57.88986 81...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Africa</td>
      <td>MULTIPOLYGON (((0.69465 5.77337, 0.63583 5.944...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South America</td>
      <td>MULTIPOLYGON (((-81.71306 12.49028, -81.72015 ...</td>
    </tr>
  </tbody>
</table>
</div>



The following code maps both your continent and meteorites into a single map.


```python
#plot coordinates on top of map graph

#this is to set the size of the borders
fig, ax = plt.subplots(1, figsize=(15,10))

#this is the map 
basemap = map_df.plot(ax=ax,cmap='Greens')

#plot coordinates on top of map graph
geo_meteor.plot(ax=basemap, color='blue', marker="o", markersize=10)

#take off axis numbers
ax.axis('off')

#put title on map
ax.set_title("NASA Meteorite Landings", fontsize=25, fontweight=3)
```




    Text(0.5, 1, 'NASA Meteorite Landings')




![png](/img/2020-05-25-NASA-Meteorite-Landings-On-Earth/output_54_1.png)



```python

```
