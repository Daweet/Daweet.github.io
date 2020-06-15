# Crimes in Montgomery County

This project aims to explore of crimes in Montgomery Count, Maryland.

The data used is downloaded and saved in the "Crime.csv" file of this repository, but can also be accessed on the Montgomery County open data website: (https://data.montgomerycountymd.gov/). Each row in the data is a crime reported by a law enforcement officer and contains the following information:

1. **Incident ID**: Police Incident Number
2. **Offence Code**: Offense_Code is the code for an offense committed within the incident as defined by the National Incident-Based Reporting System (NIBRS) of the Criminal Justice Information Services (CJIS) Division Uniform Crime Reporting (UCR) Program.
3. **CR Number**: Police Report Number
4. **Dispatch Date / Time**: The actual date and time an Officer was dispatched
5. **NIBRS Code**: FBI NBIRS codes
6. **Victims**: Number of Victims
7. **Crime Name1**: Crime against Society/Person/property or Other
8. **Crime Name2**: Describes the NBIRS_CODE
9. **Crime Name3**: Describes the OFFENSE_CODE
10. **Police District Name**: Name of District (Rockville, Wheaton etc.)
11. **Block Address**: Address in 100 block level
12. **City**: City
13. **State**: State
14. **Zip Code**: Zip code
15. **Agency**: Assigned Police Department
16. **Place**: Place description
17. **Sector**: Police Sector Name, a subset of District
18. **Beat**: Police patrol area subset within District
19. **PRA**: Police patrol area, a subset of Sector
20. **Address Number** House or Bussines Number
21. **Street Prefix** North, South, East, West
22. **Street Name** Street Name
23. **Street Suffix** Quadrant(NW, SW, etc)
24. **Street Type** Avenue, Drive, Road, etc
25. **Start_Date_Time**: Occurred from date/time
26. **End_Date_Time**: Occurred to date/time
27. **Latitude**: Latitude
28. **Longitude**: Longitude
29. **Police District Number**: Major Police Boundary
30. **Location**: Location


Let me first import libraries and package that I will be using


```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
%matplotlib inline
sns.set(style="white")
```

# Step-I: Data Cleaning
First thing we do is read the file using _read_csv()_ function, next we will check on the size of the data, the existence of null entries, along with birds eye view of information about the data. We do this using _shape()_,_isnull()_, _info()_ functions respectively as is done in the following cells. We learned that the dataset has 202094 rows and 30 columns. The names of the columns can be accessed using the _columns()_ function. The description of the names of the columns is given in the introduction part.


```python
mg_crimes = pd.read_csv("Crime.csv")
mg_crimes.head()
```

    /Users/ramlijufar/opt/anaconda3/lib/python3.7/site-packages/IPython/core/interactiveshell.py:3063: DtypeWarning: Columns (1,18) have mixed types.Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)





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
      <th>Incident ID</th>
      <th>Offence Code</th>
      <th>CR Number</th>
      <th>Dispatch Date / Time</th>
      <th>NIBRS Code</th>
      <th>Victims</th>
      <th>Crime Name1</th>
      <th>Crime Name2</th>
      <th>Crime Name3</th>
      <th>Police District Name</th>
      <th>...</th>
      <th>Street Prefix</th>
      <th>Street Name</th>
      <th>Street Suffix</th>
      <th>Street Type</th>
      <th>Start_Date_Time</th>
      <th>End_Date_Time</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Police District Number</th>
      <th>Location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201223224</td>
      <td>2303</td>
      <td>190002520</td>
      <td>01/16/2019 03:51:46 PM</td>
      <td>23C</td>
      <td>1</td>
      <td>Crime Against Property</td>
      <td>Shoplifting</td>
      <td>LARCENY - SHOPLIFTING</td>
      <td>WHEATON</td>
      <td>...</td>
      <td>NaN</td>
      <td>VEIRS MILL</td>
      <td>NaN</td>
      <td>RD</td>
      <td>01/16/2019 03:51:00 PM</td>
      <td>NaN</td>
      <td>39.037367</td>
      <td>-77.051662</td>
      <td>4D</td>
      <td>(39.0374, -77.0517)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>201224613</td>
      <td>2006</td>
      <td>190004310</td>
      <td>01/27/2019 06:05:56 PM</td>
      <td>200</td>
      <td>1</td>
      <td>Crime Against Property</td>
      <td>Arson</td>
      <td>ARSON - RESIDENTIAL</td>
      <td>MONTGOMERY VILLAGE</td>
      <td>...</td>
      <td>NaN</td>
      <td>GIRARD</td>
      <td>NaN</td>
      <td>ST</td>
      <td>01/27/2019 06:05:00 PM</td>
      <td>NaN</td>
      <td>39.146531</td>
      <td>-77.184940</td>
      <td>6D</td>
      <td>(39.1465, -77.1849)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>201267200</td>
      <td>1103</td>
      <td>190057412</td>
      <td>11/28/2019 06:08:02 AM</td>
      <td>11A</td>
      <td>1</td>
      <td>Crime Against Person</td>
      <td>Forcible Rape</td>
      <td>RAPE - STRONG-ARM</td>
      <td>WHEATON</td>
      <td>...</td>
      <td>NaN</td>
      <td>GEORGIA</td>
      <td>NaN</td>
      <td>AVE</td>
      <td>11/28/2019 06:08:00 AM</td>
      <td>NaN</td>
      <td>39.034255</td>
      <td>-77.049163</td>
      <td>4D</td>
      <td>(39.0343, -77.0492)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>201257255</td>
      <td>2308</td>
      <td>190044694</td>
      <td>09/18/2019 10:43:59 AM</td>
      <td>23D</td>
      <td>1</td>
      <td>Crime Against Property</td>
      <td>Theft from Building</td>
      <td>LARCENY - FROM BLDG</td>
      <td>ROCKVILLE</td>
      <td>...</td>
      <td>NaN</td>
      <td>JONES</td>
      <td>NaN</td>
      <td>LA</td>
      <td>04/01/2019 10:43:00 AM</td>
      <td>04/06/2019 11:59:00 PM</td>
      <td>39.104041</td>
      <td>-77.264460</td>
      <td>1D</td>
      <td>(39.104, -77.2645)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>201230900</td>
      <td>1399</td>
      <td>190011960</td>
      <td>03/15/2019 10:53:22 AM</td>
      <td>13B</td>
      <td>2</td>
      <td>Crime Against Person</td>
      <td>Simple Assault</td>
      <td>ASSAULT - 2ND DEGREE</td>
      <td>MONTGOMERY VILLAGE</td>
      <td>...</td>
      <td>NaN</td>
      <td>QUINCE ORCHARD</td>
      <td>NaN</td>
      <td>BLV</td>
      <td>03/15/2019 10:50:00 AM</td>
      <td>03/15/2019 10:55:00 AM</td>
      <td>39.141812</td>
      <td>-77.224489</td>
      <td>6D</td>
      <td>(39.1418, -77.2245)</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>




```python
# checking size of the dataset
mg_crimes.shape
```




    (202094, 30)




```python
# checking columns
mg_crimes.columns
```




    Index(['Incident ID', 'Offence Code', 'CR Number', 'Dispatch Date / Time',
           'NIBRS Code', 'Victims', 'Crime Name1', 'Crime Name2', 'Crime Name3',
           'Police District Name', 'Block Address', 'City', 'State', 'Zip Code',
           'Agency', 'Place', 'Sector', 'Beat', 'PRA', 'Address Number',
           'Street Prefix', 'Street Name', 'Street Suffix', 'Street Type',
           'Start_Date_Time', 'End_Date_Time', 'Latitude', 'Longitude',
           'Police District Number', 'Location'],
          dtype='object')


# Preliminary infromation about the dataset
mg_crimes.info()
We note here that there are some missing entries on the data, to get the exact number of missing information let us call the _isnull()_ function.


```python
# missing data for each column
mg_crimes.isnull().sum()
```




    Incident ID                    0
    Offence Code                   0
    CR Number                      0
    Dispatch Date / Time       55033
    NIBRS Code                     0
    Victims                        0
    Crime Name1                   71
    Crime Name2                   71
    Crime Name3                   71
    Police District Name           0
    Block Address              18923
    City                         911
    State                          0
    Zip Code                    3186
    Agency                         0
    Place                          0
    Sector                        49
    Beat                          49
    PRA                           31
    Address Number             18860
    Street Prefix             193147
    Street Name                    0
    Street Suffix             198205
    Street Type                  303
    Start_Date_Time                0
    End_Date_Time             104774
    Latitude                       0
    Longitude                      0
    Police District Number         0
    Location                       0
    dtype: int64




```python
print(f"Ratio of missing entries for column named 'Dispatch Date / Time': {mg_crimes['Dispatch Date / Time'].isnull().sum()/len(mg_crimes)}")
print(f"Ratio of missing entries for column named 'End_Date_Time': {mg_crimes['End_Date_Time'].isnull().sum()/len(mg_crimes)}")
```

    Ratio of missing entries for column named 'Dispatch Date / Time': 0.27231387374192206
    Ratio of missing entries for column named 'End_Date_Time': 0.5184419131691194


How to replace the missing values is the question for analysis, I have to options: one replacing the missing data by average and two dropping the missing information.

# Step-II: Analyzing Crime Patterns
## a) When do crimes happen?

One of the first questions I would like to raise is when do crimes most likely occur?, is there a difference in number of crimes between days of the week?, time of the day?, and month of the year? To answer this questions it is necessary to convert the time format into datetime format using the pandas _to_datetime()_ function.This parsing will help us extract the day, date, and time from the columns _Dispatch Date/ Time_, _End_Date_Time_, and _Start_Date_Time_.  
As part of taking care of the missing data obtained above, we can also make a comparison of information from these there columns and see if there is a gap in results obtained.

### Crime day (dispatch time)
To identify the day of the week with highest frequency of crimes, let us extract the day of the week from the "Dispatch Date / Time" column. To achive this, we will use the _day_name()_ function. 


```python
# Using dispatch time identify days of week
dispatch_time = pd.to_datetime(mg_crimes["Dispatch Date / Time"])
dispatch_week = dispatch_time.dt.day_name()
print(dispatch_week.value_counts())
ax = sns.barplot(x=dispatch_week.value_counts().index,y=dispatch_week.value_counts(),color='blue')
ax.set_xticklabels(ax.get_xticklabels(), rotation=40)
ax.set(ylim=(0, 26000),xlabel='Day of the week',ylabel='Frequency')
ax.set(ylabel='Frequency')
ax.set(xlabel='Day of the week')
plt.title("Crime frequency by day of week",fontsize= 16)
plt.xticks(fontsize=16)
plt.figure(figsize=(12,10))
```

    Friday       22723
    Tuesday      22631
    Wednesday    22553
    Thursday     22145
    Monday       21411
    Saturday     18742
    Sunday       16856
    Name: Dispatch Date / Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_14_2.png)



    <Figure size 864x720 with 0 Axes>



```python
((22723-16856)/((22723+16856)*0.5))*100
```




    29.64703504383638



We see that crimes are high during weekdays. Saturday, Sunday, Monday sees less crime and then crimes starts to climb up on Tuesday. The highest crime day is on Friday the lowest being on Sunday,the percentage difference between the highest and the lowest is about 30%.

Is it because weekends most people are staying at home, going to church, visit families?

### Crime time (dispatch time)
What time of the day do crimes frequently occur?


```python
# Using dispatch time identify time of the day
dispatch_hour = dispatch_time.dt.hour
print(dispatch_hour.value_counts())
ax = sns.barplot(x=dispatch_hour.value_counts().index,y=dispatch_hour.value_counts(), color='red')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 10000))
ax.set(xlim=(-1, 24))
ax.set(ylabel='Frequency')
ax.set(xlabel='Hours')
plt.title("Crime frequency by hour of day",fontsize= 16)
plt.xticks(fontsize=15)
plt.figure(figsize=(12,10))
```

    15.0    9718
    16.0    9494
    17.0    8728
    13.0    8262
    19.0    8117
    12.0    8040
    18.0    8018
    21.0    7729
    20.0    7625
    11.0    7358
    10.0    7350
    14.0    7269
    22.0    7255
    23.0    6654
    9.0     6449
    8.0     5022
    0.0     4932
    1.0     4208
    7.0     3755
    2.0     3730
    3.0     2588
    6.0     1973
    4.0     1533
    5.0     1254
    Name: Dispatch Date / Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_18_2.png)



    <Figure size 864x720 with 0 Axes>


We see that the crimes are low early in the morning and peaks up around 3pm with a little dip around 2pm (why?) and continues to steadily decrease towards midlnight.
Is it because the late afternoon (3-4pm) is high trafiic time, when people are out in the street?

### Crime month (dispatch time)


```python
# Using dispatch time identify month of the year
dispatch_month = dispatch_time.dt.month
print(dispatch_month.value_counts())
ax = sns.barplot(x=dispatch_month.value_counts().index,y=dispatch_month.value_counts(), color='green')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 15000))
ax.set(xlim=(-1, 12))
ax.set(ylabel='Frequency')
ax.set(xlabel='Months')
plt.title("Crime frequency by month of year",fontsize= 16)
plt.xticks(fontsize=15)
plt.figure(figsize=(12,10))
```

    5.0     13907
    8.0     13550
    7.0     13452
    6.0     13344
    4.0     13341
    10.0    12677
    9.0     12254
    1.0     12172
    12.0    12130
    11.0    12031
    3.0      9469
    2.0      8734
    Name: Dispatch Date / Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_21_2.png)



    <Figure size 864x720 with 0 Axes>


With exception of February and March the crimes seem to occur almost uniformly in each month. 
Is it because these months come after holidays, or weather is cold during these months?

### Crime day (End time)


```python
# Using end time identify days of week
end_time = pd.to_datetime(mg_crimes["End_Date_Time"])
end_week = end_time.dt.day_name()
print(end_week.value_counts())
ax = sns.barplot(x=end_week.value_counts().index,y=end_week.value_counts(),color='blue')
ax.set_xticklabels(ax.get_xticklabels(), rotation=40)
ax.set(ylim=(0, 16000),xlabel='Day of the week',ylabel='Frequency')
ax.set(ylabel='Frequency')
ax.set(xlabel='Day of the week')
plt.title("Crime frequency by day of week",fontsize= 16)
plt.xticks(fontsize=16)
plt.figure(figsize=(12,10))
```

    Friday       14998
    Monday       14520
    Wednesday    14517
    Thursday     14247
    Tuesday      14160
    Saturday     13093
    Sunday       11785
    Name: End_Date_Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_24_2.png)



    <Figure size 864x720 with 0 Axes>


We still see that occurence of crimes during weekends is lower than the weekdays, and Friday is highest crime day. 

### Crime time (End time)


```python
# Using end time identify time of the day
end_hour = end_time.dt.hour
print(end_hour.value_counts())
ax = sns.barplot(x=end_hour.value_counts().index,y=end_hour.value_counts(), color='red')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 9000))
ax.set(xlim=(-1, 24))
ax.set(ylabel='Frequency')
ax.set(xlabel='Hours')
plt.title("Crime frequency by hour of day",fontsize= 16)
plt.xticks(fontsize=15)
plt.figure(figsize=(12,10))
```

    0.0     8013
    12.0    5498
    8.0     5275
    9.0     4924
    7.0     4871
    15.0    4802
    23.0    4612
    14.0    4600
    17.0    4550
    13.0    4534
    16.0    4523
    10.0    4384
    11.0    4309
    18.0    4093
    19.0    3757
    22.0    3671
    20.0    3590
    21.0    3496
    6.0     3362
    1.0     2432
    2.0     2346
    5.0     2042
    3.0     1967
    4.0     1669
    Name: End_Date_Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_27_2.png)



    <Figure size 864x720 with 0 Axes>


Highest crime recorded during midnight(why?), morning times (8-10am and at noon) see high crime occurence. 

### Crime month (End time)


```python
# Using end time identify month of the year
end_month = end_time.dt.month
print(end_month.value_counts())
ax = sns.barplot(x=end_month.value_counts().index,y=end_month.value_counts(), color='green')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 10000))
ax.set(xlim=(-1, 12))
ax.set(ylabel='Frequency')
ax.set(xlabel='Months')
plt.title("Crime frequency by month of year",fontsize= 16)
plt.xticks(fontsize=15)
plt.figure(figsize=(12,10))
```

    8.0     9376
    10.0    9153
    7.0     9018
    9.0     8962
    11.0    8649
    12.0    8639
    1.0     8174
    3.0     7983
    2.0     7707
    4.0     6659
    5.0     6583
    6.0     6417
    Name: End_Date_Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_30_2.png)



    <Figure size 864x720 with 0 Axes>


Lowest crime months are now seem to be April, May, June

### Crime day (Start time)


```python
# Using start time identify days of week
start_time = pd.to_datetime(mg_crimes["Start_Date_Time"])
start_week = start_time.dt.day_name()
print(start_week.value_counts())
ax = sns.barplot(x=start_week.value_counts().index,y=start_week.value_counts(),color='blue')
ax.set_xticklabels(ax.get_xticklabels(), rotation=40)
ax.set(ylim=(0, 36000),xlabel='Day of the week',ylabel='Frequency')
ax.set(ylabel='Frequency')
ax.set(xlabel='Day of the week')
plt.title("Crime frequency by day of week",fontsize= 16)
plt.xticks(fontsize=16)
plt.figure(figsize=(12,10))
```

    Friday       32268
    Wednesday    30155
    Thursday     30063
    Tuesday      29766
    Monday       27603
    Saturday     27396
    Sunday       24843
    Name: Start_Date_Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_33_2.png)



    <Figure size 864x720 with 0 Axes>


We still see that occurence of crimes during weekends is lower than the weekdays, and Friday is highest crime day. 

### Crime time (Start time)


```python
# Using start time identify time of the day
start_hour = start_time.dt.hour
print(start_hour.value_counts())
ax = sns.barplot(x=start_hour.value_counts().index,y=start_hour.value_counts(), color='red')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 15000))
ax.set(xlim=(-1, 24))
ax.set(ylabel='Frequency')
ax.set(xlabel='Hours')
plt.title("Crime frequency by hour of day",fontsize= 16)
plt.xticks(fontsize=15)
plt.figure(figsize=(12,10))
```

    0     13980
    18    11907
    17    11859
    12    11831
    20    11454
    16    11446
    22    11436
    15    11398
    21    11394
    19    11353
    23    10538
    14     9431
    13     9090
    11     7936
    10     7288
    9      7194
    1      6559
    2      5761
    8      5488
    3      4067
    7      3695
    4      2538
    6      2475
    5      1976
    Name: Start_Date_Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_36_2.png)



    <Figure size 864x720 with 0 Axes>


Highest crime recorded during midnight(why?), afternoon times see high crime occurence. 

### Crime month (Start time)


```python
# Using start time identify month of the year
start_month = start_time.dt.month
print(start_month.value_counts())
ax = sns.barplot(x=start_month.value_counts().index,y=start_month.value_counts(), color='green')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 20000))
ax.set(xlim=(-1, 12))
ax.set(ylabel='Frequency')
ax.set(xlabel='Months')
plt.title("Crime frequency by month of year",fontsize= 16)
plt.xticks(fontsize=15)
plt.figure(figsize=(12,10))
```

    10    18758
    7     18479
    8     18408
    9     18110
    11    17778
    12    17643
    3     17284
    1     17125
    2     16465
    4     14237
    5     14161
    6     13646
    Name: Start_Date_Time, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_39_2.png)



    <Figure size 864x720 with 0 Axes>


Lowest crime months are now seem to be April, May, June

Comparing dispatcher, end, and start times:
We have obtained on cleaning data stage that we have zero missing entries for start_date_time column and it will be used as 'standard' data against which we contrast the results obtained from dispatch and end time calculations.

## Crime by year

Let us see if there is a trend on crime rate by year.  


```python
# Using start time identify crime by year
start_year = start_time.dt.year
print(start_year.value_counts())
ax = sns.barplot(x=start_year.value_counts().index,y=start_year.value_counts())
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
ax.set(ylim=(0, 60000))
ax.set(xlim=(-1, 7))
ax.set(ylabel='Frequency')
ax.set(xlabel='Year')
plt.title("Crime frequency by year",fontsize= 16)
plt.xticks(fontsize=15)
plt.savefig('crime_year.png')
plt.figure(figsize=(20,10))
```

    2017    56202
    2018    54164
    2019    50973
    2016    28208
    2020    12547
    Name: Start_Date_Time, dtype: int64





    <Figure size 1440x720 with 0 Axes>




![png](output_43_2.png)



    <Figure size 1440x720 with 0 Axes>


We see here that the the crime count of year 2016 and 2020 are small, of course the year 2020 is still in progress so that is why crimes are low for this year. But the crime record of 2016 needs further investigation, but I suspect it is because of missing values.  

# Can we combine all info in one plot?


```python
df_proj1=dispatch_week.value_counts()
df_proj2=end_week.value_counts()
df_proj3=start_week.value_counts()
groups = [df_proj1,df_proj2,df_proj3]
group_labels = ['Dispatch', 'End time','Start time']
colors_list = ['blue', 'red', 'green']

# Convert data to pandas DataFrame.
df = pd.DataFrame(groups, index=group_labels).T

# Plot.
pd.concat(
    [df_proj1, df_proj2, 
     df_proj3],
    axis=1).plot.bar(title='Comparison of weekly crimes',grid=False,width=0.8,figsize=(20, 8), color=colors_list).legend(bbox_to_anchor=(1.1, 1))
plt.ylabel('Frequency',fontsize=25)
plt.xlabel('Days of week',fontsize=25)
plt.xticks(fontsize=20)
plt.savefig('crime_day.png')
plt.figure(figsize=(12,8))
```




    <Figure size 864x576 with 0 Axes>




![png](output_46_1.png)



    <Figure size 864x576 with 0 Axes>



```python
df_proj1=dispatch_hour.value_counts()
df_proj2=end_hour.value_counts()
df_proj3=start_hour.value_counts()
groups = [df_proj1,df_proj2,df_proj3]
group_labels = ['Dispatch', 'End time','Start time']
colors_list = ['blue', 'red', 'green']

# Convert data to pandas DataFrame.
df = pd.DataFrame(groups, index=group_labels).T

# Plot.
pd.concat(
    [df_proj1, df_proj2, 
     df_proj3],
    axis=1).plot.bar(title='Comparison of hourly crimes',grid=False,width=0.8,figsize=(20, 8), color=colors_list).legend(bbox_to_anchor=(1.1, 1))
plt.ylabel('Frequency',fontsize=25)
plt.xlabel('Hour',fontsize=25)
plt.xticks(fontsize=20)
plt.savefig('crime_hour.png', dpi=100)
plt.figure(figsize=(12,10))
```




    <Figure size 864x720 with 0 Axes>




![png](output_47_1.png)



    <Figure size 864x720 with 0 Axes>



```python
df_proj1=dispatch_month.value_counts()
df_proj2=end_month.value_counts()
df_proj3=start_month.value_counts()
groups = [df_proj1,df_proj2,df_proj3]
group_labels = ['Dispatch', 'End time','Start time']
colors_list = ['blue', 'red', 'green']

# Convert data to pandas DataFrame.
df = pd.DataFrame(groups, index=group_labels).T

# Plot.
pd.concat(
    [df_proj1, df_proj2, 
     df_proj3],
    axis=1).plot.bar(title='Comparison of crimes by month',grid=False,width=0.8,figsize=(20, 8), color=colors_list).legend(bbox_to_anchor=(1.1, 1))
plt.ylabel('Frequency',fontsize=25)
plt.xlabel('Months',fontsize=25)
plt.xticks(fontsize=20)
plt.savefig('crime_month.png', dpi=100)
plt.figure(figsize=(12,10))
```




    <Figure size 864x720 with 0 Axes>




![png](output_48_1.png)



    <Figure size 864x720 with 0 Axes>


## b) Where do crimes happen?

The second question I would like to ask is where do crimes most likely be comitted?, is there a difference in number of crimes between cities? if so which cities are safe and which aren't? To answer these questions we need to seek information of location from the data set. In doing so we see that we have different columns that yields information about location, i.e Police District Name, Block Address, Zip Code, Sector, Beat, Latitude, Longitude, Police District Number, Location, Address Number.  
Based on the _isnull()_ assessment I made at the data cleaning stage, I chose the Police District Name column to do analysis, as this column has zero null entry.
We need to drop rows with null values and then hroup the crime occurence by Police District Name to areas with the highest crime frequencies. .


```python
# high crime area Police District Name
mg_crimes_filtered = mg_crimes.dropna(axis=0, subset=['Police District Name'])
mg_crimes_area = mg_crimes_filtered.groupby('Police District Name')['Police District Name'].count()
mg_crimes_sorted = mg_crimes_area.sort_values(ascending=False, inplace=False)
print(mg_crimes_sorted)
ax = sns.barplot(x=mg_crimes_sorted.index,y=mg_crimes_sorted, color="blue")
ax.set_ylabel('Crimes')
ax.set_xlabel('Police District')
ax.set_xticklabels(ax.get_xticklabels(), rotation=90)
plt.title("Crime occurence by area",fontsize= 16)
plt.xticks(fontsize=15)
plt.savefig('crime_bydistrict.png', dpi=100)
plt.figure(figsize=(12,10))
```

    Police District Name
    SILVER SPRING          43340
    WHEATON                39583
    MONTGOMERY VILLAGE     34193
    ROCKVILLE              27536
    BETHESDA               26810
    GERMANTOWN             25679
    CITY OF TAKOMA PARK     4904
    OTHER                     36
    TAKOMA PARK               13
    Name: Police District Name, dtype: int64





    <Figure size 864x720 with 0 Axes>




![png](output_50_2.png)



    <Figure size 864x720 with 0 Axes>


We see that Silver Spring is where the highest crime occurence is registered, with in this area we can further refine to identify the cities associated with the highest crimes

## c) What type of crimes?

Information related to the types of crimes involved are provided in columns 'Crime Name1','Crime Name2','Crime Name3'. As these columns have same amount of missing data, i.e 71, there is no preference to chose one from the other based on null analysis. But looking at the description and a view on the columns I found the third column 'Crime Name3' to be somewhat broader and thus I use it to make my analysis. As a future endeveaor it will be good to make further analysis based on 'Crime Name1' and 'Crime Name2' and make a comparison.
The question I try to answer at this stage is: which types of crimes are commonly occuring? In order to identify the types of crimes I need to pivote table 'Crime Name3' and count the frequency of 'Incident ID'.


```python
# Identifying the most recurring types of crimes
crime_types = mg_crimes.pivot_table(index='Crime Name3',values=['Incident ID'],aggfunc='count')
crime_types.rename(columns={'Incident ID':'Frequency'}, inplace=True)
crime_types.reset_index(inplace=True)
sorted_crime_types_top= crime_types.sort_values(by='Frequency', ascending=False).head()
sorted_crime_types_bottom= crime_types.sort_values(by='Frequency', ascending=True).head()
```


```python
print(f"Top five types of crimes")
sorted_crime_types_top

```

    Top five types of crimes





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
      <th>Crime Name3</th>
      <th>Frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>187</th>
      <td>LARCENY - FROM AUTO</td>
      <td>16811</td>
    </tr>
    <tr>
      <th>90</th>
      <td>DRUGS - MARIJUANA - POSSESS</td>
      <td>13983</td>
    </tr>
    <tr>
      <th>239</th>
      <td>POLICE INFORMATION</td>
      <td>11194</td>
    </tr>
    <tr>
      <th>68</th>
      <td>DRIVING UNDER THE INFLUENCE LIQUOR</td>
      <td>10878</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ASSAULT - 2ND DEGREE</td>
      <td>10858</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(f"Least five types of crimes ")
sorted_crime_types_bottom
```

    Least five types of crimes 





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
      <th>Crime Name3</th>
      <th>Frequency</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>161</th>
      <td>HOMICIDE - NEGLIGENT MANSLAUGHTER</td>
      <td>1</td>
    </tr>
    <tr>
      <th>52</th>
      <td>COMPOUNDING CRIME</td>
      <td>1</td>
    </tr>
    <tr>
      <th>53</th>
      <td>CONDIT RELEASE VIOLATION</td>
      <td>1</td>
    </tr>
    <tr>
      <th>54</th>
      <td>CONSERVATION - ANIMALS (DESCRIBE OFFENSE)</td>
      <td>1</td>
    </tr>
    <tr>
      <th>62</th>
      <td>DAMAGE PROPERTY - BUSINESS-WITH EXPLOSIVE</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### Relation between crime types and district and area?


```python
crimes_district = mg_crimes.pivot_table(index=['Police District Name', 'Crime Name3'], values=['Incident ID'], aggfunc='count')
crimes_district.rename(columns={'Incident ID':'Count'}, inplace=True) # Renaming column
crimes_district.reset_index(inplace=True) # Removing indexes

idx = crimes_district.groupby(['Police District Name'])['Count'].transform(max) == crimes_district['Count']
crimes_district[idx]
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
      <th>Police District Name</th>
      <th>Crime Name3</th>
      <th>Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>129</th>
      <td>BETHESDA</td>
      <td>LARCENY - FROM AUTO</td>
      <td>3046</td>
    </tr>
    <tr>
      <th>330</th>
      <td>CITY OF TAKOMA PARK</td>
      <td>LARCENY - FROM AUTO</td>
      <td>615</td>
    </tr>
    <tr>
      <th>556</th>
      <td>GERMANTOWN</td>
      <td>LARCENY - SHOPLIFTING</td>
      <td>2163</td>
    </tr>
    <tr>
      <th>797</th>
      <td>MONTGOMERY VILLAGE</td>
      <td>LARCENY - FROM AUTO</td>
      <td>2470</td>
    </tr>
    <tr>
      <th>930</th>
      <td>OTHER</td>
      <td>POLICE INFORMATION</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1111</th>
      <td>ROCKVILLE</td>
      <td>POLICE INFORMATION</td>
      <td>2541</td>
    </tr>
    <tr>
      <th>1330</th>
      <td>SILVER SPRING</td>
      <td>LARCENY - FROM AUTO</td>
      <td>4259</td>
    </tr>
    <tr>
      <th>1449</th>
      <td>TAKOMA PARK</td>
      <td>DRUGS - MARIJUANA - POSSESS</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1522</th>
      <td>WHEATON</td>
      <td>DRUGS - MARIJUANA - POSSESS</td>
      <td>3503</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Ordering top to bottom
crimes_district.sort_values(by=['Count'], ascending=False)[idx]
```

    /Users/ramlijufar/opt/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:2: UserWarning: Boolean Series key will be reindexed to match DataFrame index.
      





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
      <th>Police District Name</th>
      <th>Crime Name3</th>
      <th>Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1330</th>
      <td>SILVER SPRING</td>
      <td>LARCENY - FROM AUTO</td>
      <td>4259</td>
    </tr>
    <tr>
      <th>1522</th>
      <td>WHEATON</td>
      <td>DRUGS - MARIJUANA - POSSESS</td>
      <td>3503</td>
    </tr>
    <tr>
      <th>129</th>
      <td>BETHESDA</td>
      <td>LARCENY - FROM AUTO</td>
      <td>3046</td>
    </tr>
    <tr>
      <th>1111</th>
      <td>ROCKVILLE</td>
      <td>POLICE INFORMATION</td>
      <td>2541</td>
    </tr>
    <tr>
      <th>797</th>
      <td>MONTGOMERY VILLAGE</td>
      <td>LARCENY - FROM AUTO</td>
      <td>2470</td>
    </tr>
    <tr>
      <th>556</th>
      <td>GERMANTOWN</td>
      <td>LARCENY - SHOPLIFTING</td>
      <td>2163</td>
    </tr>
    <tr>
      <th>330</th>
      <td>CITY OF TAKOMA PARK</td>
      <td>LARCENY - FROM AUTO</td>
      <td>615</td>
    </tr>
    <tr>
      <th>930</th>
      <td>OTHER</td>
      <td>POLICE INFORMATION</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1449</th>
      <td>TAKOMA PARK</td>
      <td>DRUGS - MARIJUANA - POSSESS</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



### Which of places/areas (street,parking, mall etc?) do we frequently see crimes


```python
crimes_place = mg_crimes.pivot_table(index=['Place'], values=['Incident ID'], aggfunc='count')
crimes_place.rename(columns={'Incident ID':'Count'}, inplace=True) 
crimes_place.reset_index(inplace=True) 

top_places = crimes_place[crimes_place['Count']>1000].sort_values(by=['Count'], ascending=False)
top_places
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
      <th>Place</th>
      <th>Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>78</th>
      <td>Street - In vehicle</td>
      <td>26793</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Residence - Single Family</td>
      <td>20919</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Residence - Apartment/Condo</td>
      <td>17610</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Other/Unknown</td>
      <td>13676</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Street - Residential</td>
      <td>13001</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Parking Lot - Residential</td>
      <td>9327</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Residence -Townhouse/Duplex</td>
      <td>9119</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Parking Lot - Commercial</td>
      <td>7367</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Residence - Driveway</td>
      <td>6995</td>
    </tr>
    <tr>
      <th>74</th>
      <td>School/College</td>
      <td>5912</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Street - Commercial</td>
      <td>5608</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Retail - Department/Discount Store</td>
      <td>5376</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Street - Other</td>
      <td>4135</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Retail - Mall</td>
      <td>4095</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Restaurant</td>
      <td>4064</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Grocery/Supermarket</td>
      <td>3416</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Residence - Other</td>
      <td>2734</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Retail - Other</td>
      <td>2705</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Commercial - Office Building</td>
      <td>2314</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Government Building</td>
      <td>2196</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Residence - Yard</td>
      <td>2116</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Convenience Store</td>
      <td>2060</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Street - Bus Stop</td>
      <td>1767</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Hotel/Motel/Etc.</td>
      <td>1579</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Hospital/Emergency Care Center</td>
      <td>1441</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Parking Garage - Residential</td>
      <td>1415</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Gas Station</td>
      <td>1336</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Retail - Clothing</td>
      <td>1232</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Park</td>
      <td>1229</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Parking Lot - Other</td>
      <td>1197</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Retail - Drug Store/Pharmacy</td>
      <td>1149</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Parking Garage - Commercial</td>
      <td>1113</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Parking Garage - County</td>
      <td>1036</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Bar/Night Club</td>
      <td>1036</td>
    </tr>
  </tbody>
</table>
</div>




```python

ax = top_places.plot.barh (x='Place',y='Count',figsize=(10, 6), color='red')
plt.xlabel('Frequency') # add to x-label to the plot
plt.ylabel('Crime places') # add y-label to the plot
plt.title('Crime places by frequency') # add title to the plot
plt.savefig('crime_types.png', dpi=100)
```


![png](output_62_0.png)



# Conclusions

* For dispatch time analysis, three factors were used: hour, day of the week and month. According to the hours chart, most crimes occur during the morning (7am to 12pm). The beginning of the week was also the period most frequently (Monday, Tuesday and Wednesday). The analysis of the months shows that February and March, have lesser crime incidents.
* Most of the crimes committed are indicated to be committed in and around Silver Spring district.
* We also observed that the most crimes occur around residential places, and least crimes are commited in places like Bar/Club.
* The most common crime is Larceny followed by drugs/marijuana possession.
* The least crimes are crimes such as: homicide, damage property, etc.

# Further exploration needed on...

From the data it is still possible to dig deep and explore more parameters of which I cite the following ideas :

* Is it possible to determine the longest and shortest police response time? Does the response time vary from district to district?
* It will be insightful and helpful to classify the types of crimes by whether they are violent or not.
* Furthermore, a relevant information could be to know in which places there are more occurrences of crimes and in which shift these crimes are most common.
* Further analysis based on year is needed to answer the low crime level of year 2016.
* Will we arrive at different conclusion if we make analysis based on population density instead of district?
