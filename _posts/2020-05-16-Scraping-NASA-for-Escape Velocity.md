---
layout: post
title: "Scraping NASA for Escape Velocity"
category: Data Science, Physics
tags: [gravity, Beautiful Soup, table tag, Stacked bar plot]
date: 2020-05-16
---

If you want to leave Earth and fly away, you need to get rid of the influence of gravity. For that it happen you need a critical velocity with which you are able to overcome Earth's gravitational pull. In physics this velocity is called as escape velocity.  

One way to calculate this escape velocity is using total mechanical energy. Consideration of total mechanical energy of the isolated system dictates that $$\Delta E=\Delta K + \Delta U_g=0$$. This inturn implies that $$E_i=E_f$$, recalling that Kinetic energy is $$K=\frac{1}{2}mv^2$$ and gravitational energy is given by $$U_g=-\frac{GMm}{r}$$, it then follows that.

$$\frac{1}{2}mv_i^2-\frac{GMm}{r_i}=\frac{1}{2}mv_f^2-\frac{GMm}{r_f}$$

Therefore escape velocity can be calculated as
$$\frac{1}{2}mv_i^2-\frac{GM_em}{R_e}=-\frac{GM_em}{r_{max}}$$  
$$v_i^2=2GM_e\big(\frac{1}{R_e}-\frac{1}{r_{max}}\big)$$

If it escapes $$r_{max}\rightarrow\infty$$ leading to

$$v_{esc}=\sqrt{\frac{2GM_e}{R_e}}$$

You can see that the result is independent of the object's mass. Let us now import the necessary library and values for gravitational constant, mass of Earth, radius of Earth.

We can calculate the escape velocity from each of the planets in our solar system, all we need to need is plug the value of mass and radius of respective planets. Then again we can find these information from NASA's page, and as it would be fun to do so we opt to go to NASA'a page and scrap this information (and more actually). 


```python
%matplotlib inline
import numpy as np 
import matplotlib.pyplot as plt 
from math import *
import seaborn as sns
from sklearn.preprocessing import MinMaxScaler
```


```python
G = 6.674 *10**(-11)
M_e = 5.97*10**(24)
R_e = 6.37*10**6

def escape_velocity():
    print (sqrt(2*G*M_e/R_e))
```


```python
escape_velocity()
```

    11184.731126006896


This is the velocity required to escape Earth's gravitional force.

# Web Scraping Data from HTML

We can scrap the information about planets from the following [planetary]("https://nssdc.gsfc.nasa.gov/planetary/factsheet/planet_table_ratio.html"), it is provided in a table form. The table is normalized. We set the values of Earth to 1 and the other entries are expressed in comparison to Earth's values.


```python
import requests #library used to download web pages.
#specify the url
URL = "https://nssdc.gsfc.nasa.gov/planetary/factsheet/planet_table_ratio.html"
```


```python
# The GET method indicates that you’re trying to get or retrieve data from a specified resource. 
# Connect to the website using the variable 'page'
# To make a GET request, invoke requests.get().
page = requests.get(URL)
```


```python
# A Response is a powerful object for inspecting the results of the request.
type(page)
```




    requests.models.Response




```python
# verify successful connection to website

# To know about the all codes 
# https://www.restapitutorial.com/httpstatuscodes.html
  
#  a 200 OK status means that your request was successful,and the server responded with the data you were requesting,
# whereas a 404 NOT FOUND status means that the resource you were looking for was not found.     
page.status_code
```




    200




```python
#save string format of website HTML into a variable
HTMLstr = page.text
print(HTMLstr[:1000])
```

    <html>
    <head>
    <title>Planetary Fact Sheet - Ratio to Earth</title>
    </head>
    <body bgcolor=FFFFFF>
    <p>                                                                                                                                                                                                                        <!-- text and html formatting by drw at n.s.s.d c -->
    <hr>
    <H1>Planetary Fact Sheet - Ratio to Earth Values</H1>
    <hr>
    <p>
    
    <table role="presentation" border="2" cellspacing="1" cellpadding="4">
    <tr>
      <td align=left><b>&nbsp;</b></td>
      <td align=center bgcolor=F5F5F5><b>&nbsp;<a href="mercuryfact.html">MERCURY</a>&nbsp;</b></td>
      <td align=center><b>&nbsp;<a href="venusfact.html">VENUS</a>&nbsp;</b></td>
      <td align=center bgcolor=F5F5F5><b>&nbsp;<a href="earthfact.html">EARTH</a>&nbsp;</b></td>
      <td align=center><b>&nbsp;<a href="moonfact.html">MOON</a>&nbsp;</b></td>
      <td align=center bgcolor=F5F5F5><b>&nbsp;<a href="marsfact.html">MARS</a>&nbsp;</b></td>
      <td align=ce



```python
#import the Beautiful soup functions to parse the data returned from the website

# Beautiful Soup is a library that makes it easy to scrape information from web pages. It sits atop an HTML
# or XML parser, providing Pythonic idioms for iterating, searching, and modifying the parse tree.
from bs4 import BeautifulSoup

```


```python

# parse the html using beautiful soup and store in variable `soup`
# First argument: It is the raw HTML content.
# Second Argument:  Specifying the HTML parser we want to use.
soup = BeautifulSoup(HTMLstr, "html.parser")
```

Format page contents to include indentation. Then soup.prettify() is printed, it gives the visual representation of the parse tree created from the raw HTML content.
```python
print (soup.prettify())
```


```python
# soup.<tag>: Return content between opening and closing tag including tag.
soup.title
```




    <title>Planetary Fact Sheet - Ratio to Earth</title>




```python
#shows the first <a> tag on the page
soup.a
```




    <a href="mercuryfact.html">MERCURY</a>



The following code finds all `<a>` tags on the page. This code finds all the `<a>`  tags in the document (you can replace b with any tag you want to find)
```python
soup.find_all("a")
```


```python
#show hyperlink reference for all <a> tags
all_links=soup.find_all("a")

#The following selects the first 20 rows
# If you want to see all, remove [0:20]
for link in all_links[0:20]:
    print (link.get("href"))
```

    mercuryfact.html
    venusfact.html
    earthfact.html
    moonfact.html
    marsfact.html
    jupiterfact.html
    saturnfact.html
    uranusfact.html
    neptunefact.html
    plutofact.html
    planetfact_ratio_notes.html#mass
    planetfact_ratio_notes.html#diam
    planetfact_ratio_notes.html#dens
    planetfact_ratio_notes.html#grav
    planetfact_ratio_notes.html#escv
    planetfact_ratio_notes.html#rotp
    planetfact_ratio_notes.html#leng
    planetfact_ratio_notes.html#dist
    planetfact_ratio_notes.html#peri
    planetfact_ratio_notes.html#peri


Next we find all the `<table>` tags using the following code, I won't display the output here as it is a long list
```python
all_tables=soup.find_all('table')
all_tables
```
    
Then we extract the data we are interested in and get the `<table>` tag that contains the data we want to scrape. You can simply use
```python soup.table ``` or 

```python
planet_table=soup.find('table')
planet_table
```


```python
all_tables=soup.find_all('table')
planet_table=soup.find('table')
```

Next we set empty list to hold data for each of the columns we are reading using the following code block.


```python
#set empty lists to hold data of each column
A=[]#Physical Quantities
B=[]#Mercury
C=[]#Venus
D=[]#Earth
E=[]#Moon
F=[]#Mars
G=[]#Jupiter
H=[]#Saturn
I=[]#Uranus
J=[]#Neptune
K=[]#Pluto

#find all <tr> tags in the table and go through each one (row)
# tr table row tag
for row in planet_table.findAll("tr"):
    body=row.findAll('td') #To store second column data
    #get all the <td> tags for each <tr> tag
    cells = row.findAll('td')
    
    #if there are 11 <td> tags, 11 cells in a row
    if len(cells)==11: 
        
        A.append(cells[0].find(text=True)) #gets info in first column and adds it to list A
        B.append(cells[1].find(text=True)) # gets info of Mercury column and adds it to list B
        C.append(cells[2].find(text=True)) # gets info of Venus column and add it to list C
        D.append(cells[3].find(text=True)) # gets info of Earth and adds it to list D
        E.append(cells[4].find(text=True)) # gets info of Moon column and adds it to list E
        F.append(cells[5].find(text=True)) # gets info of Mars column and adds it to list F
        G.append(cells[6].find(text=True)) # gets info of Jupiter column tand adds it to list G
        H.append(cells[7].find(text=True)) # gets info of Saturn column tand adds to list H
        I.append(cells[8].find(text=True)) # gets info of Uranus column and adds it to list I
        J.append(cells[9].find(text=True)) # gets info of Neptune column and adds it to list J
        K.append(cells[10].find(text=True)) # gets info of NePluto column and adds to list K
```
You can print cells to reveal the out put 
> print(cells[n]) where n=0,1,...9

You should also check if the length is consistent and have imported all the required information, that can be done using

> print(len(X)) where x= A, B, ...K


```python
#verify data in list A
A
```




    ['\xa0',
     'Mass',
     'Diameter',
     'Density',
     'Gravity',
     'Escape Velocity',
     'Rotation Period',
     'Length of Day',
     'Distance from Sun',
     'Perihelion',
     'Aphelion',
     'Orbital Period',
     'Orbital Velocity',
     'Orbital Eccentricity',
     'Obliquity to Orbit',
     'Surface Pressure',
     'Number of Moons',
     'Ring System?',
     'Global Magnetic Field?',
     '\xa0']



Great! We successfully read the table and put it in a list [A,B,...,K], now we need to convert it into DataFrame


```python
#import pandas to convert list to data frame
import pandas as pd

df=pd.DataFrame(A, columns=['Physical_Measurement']) #turn list A into dataframe first

#add other lists as new columns in my new dataframe
df['Mercury'] = B
df['Venus'] = C
df['Earth'] = D
df['Moon'] = E
df['Mars'] = F
df['Jupiter'] = G
df['Saturn'] = H
df['Uranus'] = I
df['Neptune'] = J
df['Pluto'] = K

```

We then proceed on replacing null values and we also see that we have 'Unknown*'. After that we display our dataframe and inspect if everything is alright.


```python
df=df.fillna(0)
df=df.replace(to_replace = 'Unknown*', value =0) 
#Planetary Fact Sheet - Ratio to Earth Values

#show first 5 rows of created dataframe
df

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
      <th>Physical_Measurement</th>
      <th>Mercury</th>
      <th>Venus</th>
      <th>Earth</th>
      <th>Moon</th>
      <th>Mars</th>
      <th>Jupiter</th>
      <th>Saturn</th>
      <th>Uranus</th>
      <th>Neptune</th>
      <th>Pluto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mass</td>
      <td>0.0553</td>
      <td>0.815</td>
      <td>1</td>
      <td>0.0123</td>
      <td>0.107</td>
      <td>317.8</td>
      <td>95.2</td>
      <td>14.5</td>
      <td>17.1</td>
      <td>0.0025</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Diameter</td>
      <td>0.383</td>
      <td>0.949</td>
      <td>1</td>
      <td>0.2724</td>
      <td>0.532</td>
      <td>11.21</td>
      <td>9.45</td>
      <td>4.01</td>
      <td>3.88</td>
      <td>0.186</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Density</td>
      <td>0.984</td>
      <td>0.951</td>
      <td>1</td>
      <td>0.605</td>
      <td>0.713</td>
      <td>0.240</td>
      <td>0.125</td>
      <td>0.230</td>
      <td>0.297</td>
      <td>0.380</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gravity</td>
      <td>0.378</td>
      <td>0.907</td>
      <td>1</td>
      <td>0.166</td>
      <td>0.377</td>
      <td>2.36</td>
      <td>0.916</td>
      <td>0.889</td>
      <td>1.12</td>
      <td>0.071</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Escape Velocity</td>
      <td>0.384</td>
      <td>0.926</td>
      <td>1</td>
      <td>0.213</td>
      <td>0.450</td>
      <td>5.32</td>
      <td>3.17</td>
      <td>1.90</td>
      <td>2.10</td>
      <td>0.116</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Rotation Period</td>
      <td>58.8</td>
      <td>-244</td>
      <td>1</td>
      <td>27.4</td>
      <td>1.03</td>
      <td>0.415</td>
      <td>0.445</td>
      <td>-0.720</td>
      <td>0.673</td>
      <td>6.41</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Length of Day</td>
      <td>175.9</td>
      <td>116.8</td>
      <td>1</td>
      <td>29.5</td>
      <td>1.03</td>
      <td>0.414</td>
      <td>0.444</td>
      <td>0.718</td>
      <td>0.671</td>
      <td>6.39</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance from Sun</td>
      <td>0.387</td>
      <td>0.723</td>
      <td>1</td>
      <td>0.00257*</td>
      <td>1.52</td>
      <td>5.20</td>
      <td>9.58</td>
      <td>19.20</td>
      <td>30.05</td>
      <td>39.48</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Perihelion</td>
      <td>0.313</td>
      <td>0.731</td>
      <td>1</td>
      <td>0.00247*</td>
      <td>1.41</td>
      <td>5.03</td>
      <td>9.20</td>
      <td>18.64</td>
      <td>30.22</td>
      <td>30.16</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Aphelion</td>
      <td>0.459</td>
      <td>0.716</td>
      <td>1</td>
      <td>0.00267*</td>
      <td>1.64</td>
      <td>5.37</td>
      <td>9.96</td>
      <td>19.75</td>
      <td>29.89</td>
      <td>48.49</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Orbital Period</td>
      <td>0.241</td>
      <td>0.615</td>
      <td>1</td>
      <td>0.0748*</td>
      <td>1.88</td>
      <td>11.9</td>
      <td>29.4</td>
      <td>83.7</td>
      <td>163.7</td>
      <td>247.9</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Orbital Velocity</td>
      <td>1.59</td>
      <td>1.18</td>
      <td>1</td>
      <td>0.0343*</td>
      <td>0.808</td>
      <td>0.439</td>
      <td>0.325</td>
      <td>0.228</td>
      <td>0.182</td>
      <td>0.157</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Orbital Eccentricity</td>
      <td>12.3</td>
      <td>0.401</td>
      <td>1</td>
      <td>3.29</td>
      <td>5.60</td>
      <td>2.93</td>
      <td>3.38</td>
      <td>2.74</td>
      <td>0.677</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Obliquity to Orbit</td>
      <td>0.001</td>
      <td>0.113*</td>
      <td>1</td>
      <td>0.285</td>
      <td>1.07</td>
      <td>0.134</td>
      <td>1.14</td>
      <td>4.17*</td>
      <td>1.21</td>
      <td>2.45*</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Surface Pressure</td>
      <td>0</td>
      <td>92</td>
      <td>1</td>
      <td>0</td>
      <td>0.01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00001</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Number of Moons</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>79</td>
      <td>82</td>
      <td>27</td>
      <td>14</td>
      <td>5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Ring System?</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Global Magnetic Field?</td>
      <td>Yes</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Unknown</td>
    </tr>
    <tr>
      <th>19</th>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



First we notice that we have an empty row on the first and last rows of the dataframe. 
Let us drop these two using the following code.


```python
df=df.drop(df.index[0])
df=df.drop(df.index[-1])
df
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
      <th>Physical_Measurement</th>
      <th>Mercury</th>
      <th>Venus</th>
      <th>Earth</th>
      <th>Moon</th>
      <th>Mars</th>
      <th>Jupiter</th>
      <th>Saturn</th>
      <th>Uranus</th>
      <th>Neptune</th>
      <th>Pluto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Mass</td>
      <td>0.0553</td>
      <td>0.815</td>
      <td>1</td>
      <td>0.0123</td>
      <td>0.107</td>
      <td>317.8</td>
      <td>95.2</td>
      <td>14.5</td>
      <td>17.1</td>
      <td>0.0025</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Diameter</td>
      <td>0.383</td>
      <td>0.949</td>
      <td>1</td>
      <td>0.2724</td>
      <td>0.532</td>
      <td>11.21</td>
      <td>9.45</td>
      <td>4.01</td>
      <td>3.88</td>
      <td>0.186</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Density</td>
      <td>0.984</td>
      <td>0.951</td>
      <td>1</td>
      <td>0.605</td>
      <td>0.713</td>
      <td>0.240</td>
      <td>0.125</td>
      <td>0.230</td>
      <td>0.297</td>
      <td>0.380</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gravity</td>
      <td>0.378</td>
      <td>0.907</td>
      <td>1</td>
      <td>0.166</td>
      <td>0.377</td>
      <td>2.36</td>
      <td>0.916</td>
      <td>0.889</td>
      <td>1.12</td>
      <td>0.071</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Escape Velocity</td>
      <td>0.384</td>
      <td>0.926</td>
      <td>1</td>
      <td>0.213</td>
      <td>0.450</td>
      <td>5.32</td>
      <td>3.17</td>
      <td>1.90</td>
      <td>2.10</td>
      <td>0.116</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Rotation Period</td>
      <td>58.8</td>
      <td>-244</td>
      <td>1</td>
      <td>27.4</td>
      <td>1.03</td>
      <td>0.415</td>
      <td>0.445</td>
      <td>-0.720</td>
      <td>0.673</td>
      <td>6.41</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Length of Day</td>
      <td>175.9</td>
      <td>116.8</td>
      <td>1</td>
      <td>29.5</td>
      <td>1.03</td>
      <td>0.414</td>
      <td>0.444</td>
      <td>0.718</td>
      <td>0.671</td>
      <td>6.39</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance from Sun</td>
      <td>0.387</td>
      <td>0.723</td>
      <td>1</td>
      <td>0.00257*</td>
      <td>1.52</td>
      <td>5.20</td>
      <td>9.58</td>
      <td>19.20</td>
      <td>30.05</td>
      <td>39.48</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Perihelion</td>
      <td>0.313</td>
      <td>0.731</td>
      <td>1</td>
      <td>0.00247*</td>
      <td>1.41</td>
      <td>5.03</td>
      <td>9.20</td>
      <td>18.64</td>
      <td>30.22</td>
      <td>30.16</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Aphelion</td>
      <td>0.459</td>
      <td>0.716</td>
      <td>1</td>
      <td>0.00267*</td>
      <td>1.64</td>
      <td>5.37</td>
      <td>9.96</td>
      <td>19.75</td>
      <td>29.89</td>
      <td>48.49</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Orbital Period</td>
      <td>0.241</td>
      <td>0.615</td>
      <td>1</td>
      <td>0.0748*</td>
      <td>1.88</td>
      <td>11.9</td>
      <td>29.4</td>
      <td>83.7</td>
      <td>163.7</td>
      <td>247.9</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Orbital Velocity</td>
      <td>1.59</td>
      <td>1.18</td>
      <td>1</td>
      <td>0.0343*</td>
      <td>0.808</td>
      <td>0.439</td>
      <td>0.325</td>
      <td>0.228</td>
      <td>0.182</td>
      <td>0.157</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Orbital Eccentricity</td>
      <td>12.3</td>
      <td>0.401</td>
      <td>1</td>
      <td>3.29</td>
      <td>5.60</td>
      <td>2.93</td>
      <td>3.38</td>
      <td>2.74</td>
      <td>0.677</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Obliquity to Orbit</td>
      <td>0.001</td>
      <td>0.113*</td>
      <td>1</td>
      <td>0.285</td>
      <td>1.07</td>
      <td>0.134</td>
      <td>1.14</td>
      <td>4.17*</td>
      <td>1.21</td>
      <td>2.45*</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Surface Pressure</td>
      <td>0</td>
      <td>92</td>
      <td>1</td>
      <td>0</td>
      <td>0.01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00001</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Number of Moons</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>79</td>
      <td>82</td>
      <td>27</td>
      <td>14</td>
      <td>5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Ring System?</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Global Magnetic Field?</td>
      <td>Yes</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Unknown</td>
    </tr>
  </tbody>
</table>
</div>



It is always good habit to check data types, and that can be achieved via .dtypes()


```python
df.dtypes
```




    Physical_Measurement    object
    Mercury                 object
    Venus                   object
    Earth                   object
    Moon                    object
    Mars                    object
    Jupiter                 object
    Saturn                  object
    Uranus                  object
    Neptune                 object
    Pluto                   object
    dtype: object



Although we see numbers, all the data types are object type, and this is because some of the entries have ("*") at the end, let us take care of these then


```python
#getting rid of *
df= df.applymap(lambda x: x.strip('*') if isinstance(x, str) else x)
df
#display the table if everything is OK
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
      <th>Physical_Measurement</th>
      <th>Mercury</th>
      <th>Venus</th>
      <th>Earth</th>
      <th>Moon</th>
      <th>Mars</th>
      <th>Jupiter</th>
      <th>Saturn</th>
      <th>Uranus</th>
      <th>Neptune</th>
      <th>Pluto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Mass</td>
      <td>0.0553</td>
      <td>0.815</td>
      <td>1</td>
      <td>0.0123</td>
      <td>0.107</td>
      <td>317.8</td>
      <td>95.2</td>
      <td>14.5</td>
      <td>17.1</td>
      <td>0.0025</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Diameter</td>
      <td>0.383</td>
      <td>0.949</td>
      <td>1</td>
      <td>0.2724</td>
      <td>0.532</td>
      <td>11.21</td>
      <td>9.45</td>
      <td>4.01</td>
      <td>3.88</td>
      <td>0.186</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Density</td>
      <td>0.984</td>
      <td>0.951</td>
      <td>1</td>
      <td>0.605</td>
      <td>0.713</td>
      <td>0.240</td>
      <td>0.125</td>
      <td>0.230</td>
      <td>0.297</td>
      <td>0.380</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gravity</td>
      <td>0.378</td>
      <td>0.907</td>
      <td>1</td>
      <td>0.166</td>
      <td>0.377</td>
      <td>2.36</td>
      <td>0.916</td>
      <td>0.889</td>
      <td>1.12</td>
      <td>0.071</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Escape Velocity</td>
      <td>0.384</td>
      <td>0.926</td>
      <td>1</td>
      <td>0.213</td>
      <td>0.450</td>
      <td>5.32</td>
      <td>3.17</td>
      <td>1.90</td>
      <td>2.10</td>
      <td>0.116</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Rotation Period</td>
      <td>58.8</td>
      <td>-244</td>
      <td>1</td>
      <td>27.4</td>
      <td>1.03</td>
      <td>0.415</td>
      <td>0.445</td>
      <td>-0.720</td>
      <td>0.673</td>
      <td>6.41</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Length of Day</td>
      <td>175.9</td>
      <td>116.8</td>
      <td>1</td>
      <td>29.5</td>
      <td>1.03</td>
      <td>0.414</td>
      <td>0.444</td>
      <td>0.718</td>
      <td>0.671</td>
      <td>6.39</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance from Sun</td>
      <td>0.387</td>
      <td>0.723</td>
      <td>1</td>
      <td>0.00257</td>
      <td>1.52</td>
      <td>5.20</td>
      <td>9.58</td>
      <td>19.20</td>
      <td>30.05</td>
      <td>39.48</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Perihelion</td>
      <td>0.313</td>
      <td>0.731</td>
      <td>1</td>
      <td>0.00247</td>
      <td>1.41</td>
      <td>5.03</td>
      <td>9.20</td>
      <td>18.64</td>
      <td>30.22</td>
      <td>30.16</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Aphelion</td>
      <td>0.459</td>
      <td>0.716</td>
      <td>1</td>
      <td>0.00267</td>
      <td>1.64</td>
      <td>5.37</td>
      <td>9.96</td>
      <td>19.75</td>
      <td>29.89</td>
      <td>48.49</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Orbital Period</td>
      <td>0.241</td>
      <td>0.615</td>
      <td>1</td>
      <td>0.0748</td>
      <td>1.88</td>
      <td>11.9</td>
      <td>29.4</td>
      <td>83.7</td>
      <td>163.7</td>
      <td>247.9</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Orbital Velocity</td>
      <td>1.59</td>
      <td>1.18</td>
      <td>1</td>
      <td>0.0343</td>
      <td>0.808</td>
      <td>0.439</td>
      <td>0.325</td>
      <td>0.228</td>
      <td>0.182</td>
      <td>0.157</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Orbital Eccentricity</td>
      <td>12.3</td>
      <td>0.401</td>
      <td>1</td>
      <td>3.29</td>
      <td>5.60</td>
      <td>2.93</td>
      <td>3.38</td>
      <td>2.74</td>
      <td>0.677</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Obliquity to Orbit</td>
      <td>0.001</td>
      <td>0.113</td>
      <td>1</td>
      <td>0.285</td>
      <td>1.07</td>
      <td>0.134</td>
      <td>1.14</td>
      <td>4.17</td>
      <td>1.21</td>
      <td>2.45</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Surface Pressure</td>
      <td>0</td>
      <td>92</td>
      <td>1</td>
      <td>0</td>
      <td>0.01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00001</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Number of Moons</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>79</td>
      <td>82</td>
      <td>27</td>
      <td>14</td>
      <td>5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Ring System?</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Global Magnetic Field?</td>
      <td>Yes</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Unknown</td>
    </tr>
  </tbody>
</table>
</div>



Although we are not going to use it in this analysis we notice that the last two rows needs further cleaning, for example we can it from category into numeric. If you want you can also strip the question mark at the end using the strip method.


```python

df = df.replace(to_replace=['No', 'Yes','Unknown'], value=[0, 1, 2])
df
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
      <th>Physical_Measurement</th>
      <th>Mercury</th>
      <th>Venus</th>
      <th>Earth</th>
      <th>Moon</th>
      <th>Mars</th>
      <th>Jupiter</th>
      <th>Saturn</th>
      <th>Uranus</th>
      <th>Neptune</th>
      <th>Pluto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Mass</td>
      <td>0.0553</td>
      <td>0.815</td>
      <td>1</td>
      <td>0.0123</td>
      <td>0.107</td>
      <td>317.8</td>
      <td>95.2</td>
      <td>14.5</td>
      <td>17.1</td>
      <td>0.0025</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Diameter</td>
      <td>0.383</td>
      <td>0.949</td>
      <td>1</td>
      <td>0.2724</td>
      <td>0.532</td>
      <td>11.21</td>
      <td>9.45</td>
      <td>4.01</td>
      <td>3.88</td>
      <td>0.186</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Density</td>
      <td>0.984</td>
      <td>0.951</td>
      <td>1</td>
      <td>0.605</td>
      <td>0.713</td>
      <td>0.240</td>
      <td>0.125</td>
      <td>0.230</td>
      <td>0.297</td>
      <td>0.380</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gravity</td>
      <td>0.378</td>
      <td>0.907</td>
      <td>1</td>
      <td>0.166</td>
      <td>0.377</td>
      <td>2.36</td>
      <td>0.916</td>
      <td>0.889</td>
      <td>1.12</td>
      <td>0.071</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Escape Velocity</td>
      <td>0.384</td>
      <td>0.926</td>
      <td>1</td>
      <td>0.213</td>
      <td>0.450</td>
      <td>5.32</td>
      <td>3.17</td>
      <td>1.90</td>
      <td>2.10</td>
      <td>0.116</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Rotation Period</td>
      <td>58.8</td>
      <td>-244</td>
      <td>1</td>
      <td>27.4</td>
      <td>1.03</td>
      <td>0.415</td>
      <td>0.445</td>
      <td>-0.720</td>
      <td>0.673</td>
      <td>6.41</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Length of Day</td>
      <td>175.9</td>
      <td>116.8</td>
      <td>1</td>
      <td>29.5</td>
      <td>1.03</td>
      <td>0.414</td>
      <td>0.444</td>
      <td>0.718</td>
      <td>0.671</td>
      <td>6.39</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Distance from Sun</td>
      <td>0.387</td>
      <td>0.723</td>
      <td>1</td>
      <td>0.00257</td>
      <td>1.52</td>
      <td>5.20</td>
      <td>9.58</td>
      <td>19.20</td>
      <td>30.05</td>
      <td>39.48</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Perihelion</td>
      <td>0.313</td>
      <td>0.731</td>
      <td>1</td>
      <td>0.00247</td>
      <td>1.41</td>
      <td>5.03</td>
      <td>9.20</td>
      <td>18.64</td>
      <td>30.22</td>
      <td>30.16</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Aphelion</td>
      <td>0.459</td>
      <td>0.716</td>
      <td>1</td>
      <td>0.00267</td>
      <td>1.64</td>
      <td>5.37</td>
      <td>9.96</td>
      <td>19.75</td>
      <td>29.89</td>
      <td>48.49</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Orbital Period</td>
      <td>0.241</td>
      <td>0.615</td>
      <td>1</td>
      <td>0.0748</td>
      <td>1.88</td>
      <td>11.9</td>
      <td>29.4</td>
      <td>83.7</td>
      <td>163.7</td>
      <td>247.9</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Orbital Velocity</td>
      <td>1.59</td>
      <td>1.18</td>
      <td>1</td>
      <td>0.0343</td>
      <td>0.808</td>
      <td>0.439</td>
      <td>0.325</td>
      <td>0.228</td>
      <td>0.182</td>
      <td>0.157</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Orbital Eccentricity</td>
      <td>12.3</td>
      <td>0.401</td>
      <td>1</td>
      <td>3.29</td>
      <td>5.60</td>
      <td>2.93</td>
      <td>3.38</td>
      <td>2.74</td>
      <td>0.677</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Obliquity to Orbit</td>
      <td>0.001</td>
      <td>0.113</td>
      <td>1</td>
      <td>0.285</td>
      <td>1.07</td>
      <td>0.134</td>
      <td>1.14</td>
      <td>4.17</td>
      <td>1.21</td>
      <td>2.45</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Surface Pressure</td>
      <td>0</td>
      <td>92</td>
      <td>1</td>
      <td>0</td>
      <td>0.01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00001</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Number of Moons</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>79</td>
      <td>82</td>
      <td>27</td>
      <td>14</td>
      <td>5</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Ring System?</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Global Magnetic Field?</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



I find it this type of data hard to visualize, so let me change the index to be the column 'Physical_Measurement'. 


```python
#change index
df = df.set_index('Physical_Measurement')
df
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
      <th>Mercury</th>
      <th>Venus</th>
      <th>Earth</th>
      <th>Moon</th>
      <th>Mars</th>
      <th>Jupiter</th>
      <th>Saturn</th>
      <th>Uranus</th>
      <th>Neptune</th>
      <th>Pluto</th>
    </tr>
    <tr>
      <th>Physical_Measurement</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mass</th>
      <td>0.0553</td>
      <td>0.815</td>
      <td>1</td>
      <td>0.0123</td>
      <td>0.107</td>
      <td>317.8</td>
      <td>95.2</td>
      <td>14.5</td>
      <td>17.1</td>
      <td>0.0025</td>
    </tr>
    <tr>
      <th>Diameter</th>
      <td>0.383</td>
      <td>0.949</td>
      <td>1</td>
      <td>0.2724</td>
      <td>0.532</td>
      <td>11.21</td>
      <td>9.45</td>
      <td>4.01</td>
      <td>3.88</td>
      <td>0.186</td>
    </tr>
    <tr>
      <th>Density</th>
      <td>0.984</td>
      <td>0.951</td>
      <td>1</td>
      <td>0.605</td>
      <td>0.713</td>
      <td>0.240</td>
      <td>0.125</td>
      <td>0.230</td>
      <td>0.297</td>
      <td>0.380</td>
    </tr>
    <tr>
      <th>Gravity</th>
      <td>0.378</td>
      <td>0.907</td>
      <td>1</td>
      <td>0.166</td>
      <td>0.377</td>
      <td>2.36</td>
      <td>0.916</td>
      <td>0.889</td>
      <td>1.12</td>
      <td>0.071</td>
    </tr>
    <tr>
      <th>Escape Velocity</th>
      <td>0.384</td>
      <td>0.926</td>
      <td>1</td>
      <td>0.213</td>
      <td>0.450</td>
      <td>5.32</td>
      <td>3.17</td>
      <td>1.90</td>
      <td>2.10</td>
      <td>0.116</td>
    </tr>
    <tr>
      <th>Rotation Period</th>
      <td>58.8</td>
      <td>-244</td>
      <td>1</td>
      <td>27.4</td>
      <td>1.03</td>
      <td>0.415</td>
      <td>0.445</td>
      <td>-0.720</td>
      <td>0.673</td>
      <td>6.41</td>
    </tr>
    <tr>
      <th>Length of Day</th>
      <td>175.9</td>
      <td>116.8</td>
      <td>1</td>
      <td>29.5</td>
      <td>1.03</td>
      <td>0.414</td>
      <td>0.444</td>
      <td>0.718</td>
      <td>0.671</td>
      <td>6.39</td>
    </tr>
    <tr>
      <th>Distance from Sun</th>
      <td>0.387</td>
      <td>0.723</td>
      <td>1</td>
      <td>0.00257</td>
      <td>1.52</td>
      <td>5.20</td>
      <td>9.58</td>
      <td>19.20</td>
      <td>30.05</td>
      <td>39.48</td>
    </tr>
    <tr>
      <th>Perihelion</th>
      <td>0.313</td>
      <td>0.731</td>
      <td>1</td>
      <td>0.00247</td>
      <td>1.41</td>
      <td>5.03</td>
      <td>9.20</td>
      <td>18.64</td>
      <td>30.22</td>
      <td>30.16</td>
    </tr>
    <tr>
      <th>Aphelion</th>
      <td>0.459</td>
      <td>0.716</td>
      <td>1</td>
      <td>0.00267</td>
      <td>1.64</td>
      <td>5.37</td>
      <td>9.96</td>
      <td>19.75</td>
      <td>29.89</td>
      <td>48.49</td>
    </tr>
    <tr>
      <th>Orbital Period</th>
      <td>0.241</td>
      <td>0.615</td>
      <td>1</td>
      <td>0.0748</td>
      <td>1.88</td>
      <td>11.9</td>
      <td>29.4</td>
      <td>83.7</td>
      <td>163.7</td>
      <td>247.9</td>
    </tr>
    <tr>
      <th>Orbital Velocity</th>
      <td>1.59</td>
      <td>1.18</td>
      <td>1</td>
      <td>0.0343</td>
      <td>0.808</td>
      <td>0.439</td>
      <td>0.325</td>
      <td>0.228</td>
      <td>0.182</td>
      <td>0.157</td>
    </tr>
    <tr>
      <th>Orbital Eccentricity</th>
      <td>12.3</td>
      <td>0.401</td>
      <td>1</td>
      <td>3.29</td>
      <td>5.60</td>
      <td>2.93</td>
      <td>3.38</td>
      <td>2.74</td>
      <td>0.677</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>Obliquity to Orbit</th>
      <td>0.001</td>
      <td>0.113</td>
      <td>1</td>
      <td>0.285</td>
      <td>1.07</td>
      <td>0.134</td>
      <td>1.14</td>
      <td>4.17</td>
      <td>1.21</td>
      <td>2.45</td>
    </tr>
    <tr>
      <th>Surface Pressure</th>
      <td>0</td>
      <td>92</td>
      <td>1</td>
      <td>0</td>
      <td>0.01</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.00001</td>
    </tr>
    <tr>
      <th>Number of Moons</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>79</td>
      <td>82</td>
      <td>27</td>
      <td>14</td>
      <td>5</td>
    </tr>
    <tr>
      <th>Ring System?</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Global Magnetic Field?</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



Let us check now if everything is inorder when it comes to data types


```python
df.dtypes
```




    Mercury    object
    Venus      object
    Earth      object
    Moon       object
    Mars       object
    Jupiter    object
    Saturn     object
    Uranus     object
    Neptune    object
    Pluto      object
    dtype: object



To make sure everything is numeric we can use pandas *to_numeric* method and apply it to our dataFrame


```python
df = df.apply(pd.to_numeric, errors='coerce')
df.dtypes
```




    Mercury    float64
    Venus      float64
    Earth        int64
    Moon       float64
    Mars       float64
    Jupiter    float64
    Saturn     float64
    Uranus     float64
    Neptune    float64
    Pluto      float64
    dtype: object




```python
df
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
      <th>Mercury</th>
      <th>Venus</th>
      <th>Earth</th>
      <th>Moon</th>
      <th>Mars</th>
      <th>Jupiter</th>
      <th>Saturn</th>
      <th>Uranus</th>
      <th>Neptune</th>
      <th>Pluto</th>
    </tr>
    <tr>
      <th>Physical_Measurement</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mass</th>
      <td>0.0553</td>
      <td>0.815</td>
      <td>1</td>
      <td>0.01230</td>
      <td>0.107</td>
      <td>317.800</td>
      <td>95.200</td>
      <td>14.500</td>
      <td>17.100</td>
      <td>0.00250</td>
    </tr>
    <tr>
      <th>Diameter</th>
      <td>0.3830</td>
      <td>0.949</td>
      <td>1</td>
      <td>0.27240</td>
      <td>0.532</td>
      <td>11.210</td>
      <td>9.450</td>
      <td>4.010</td>
      <td>3.880</td>
      <td>0.18600</td>
    </tr>
    <tr>
      <th>Density</th>
      <td>0.9840</td>
      <td>0.951</td>
      <td>1</td>
      <td>0.60500</td>
      <td>0.713</td>
      <td>0.240</td>
      <td>0.125</td>
      <td>0.230</td>
      <td>0.297</td>
      <td>0.38000</td>
    </tr>
    <tr>
      <th>Gravity</th>
      <td>0.3780</td>
      <td>0.907</td>
      <td>1</td>
      <td>0.16600</td>
      <td>0.377</td>
      <td>2.360</td>
      <td>0.916</td>
      <td>0.889</td>
      <td>1.120</td>
      <td>0.07100</td>
    </tr>
    <tr>
      <th>Escape Velocity</th>
      <td>0.3840</td>
      <td>0.926</td>
      <td>1</td>
      <td>0.21300</td>
      <td>0.450</td>
      <td>5.320</td>
      <td>3.170</td>
      <td>1.900</td>
      <td>2.100</td>
      <td>0.11600</td>
    </tr>
    <tr>
      <th>Rotation Period</th>
      <td>58.8000</td>
      <td>-244.000</td>
      <td>1</td>
      <td>27.40000</td>
      <td>1.030</td>
      <td>0.415</td>
      <td>0.445</td>
      <td>-0.720</td>
      <td>0.673</td>
      <td>6.41000</td>
    </tr>
    <tr>
      <th>Length of Day</th>
      <td>175.9000</td>
      <td>116.800</td>
      <td>1</td>
      <td>29.50000</td>
      <td>1.030</td>
      <td>0.414</td>
      <td>0.444</td>
      <td>0.718</td>
      <td>0.671</td>
      <td>6.39000</td>
    </tr>
    <tr>
      <th>Distance from Sun</th>
      <td>0.3870</td>
      <td>0.723</td>
      <td>1</td>
      <td>0.00257</td>
      <td>1.520</td>
      <td>5.200</td>
      <td>9.580</td>
      <td>19.200</td>
      <td>30.050</td>
      <td>39.48000</td>
    </tr>
    <tr>
      <th>Perihelion</th>
      <td>0.3130</td>
      <td>0.731</td>
      <td>1</td>
      <td>0.00247</td>
      <td>1.410</td>
      <td>5.030</td>
      <td>9.200</td>
      <td>18.640</td>
      <td>30.220</td>
      <td>30.16000</td>
    </tr>
    <tr>
      <th>Aphelion</th>
      <td>0.4590</td>
      <td>0.716</td>
      <td>1</td>
      <td>0.00267</td>
      <td>1.640</td>
      <td>5.370</td>
      <td>9.960</td>
      <td>19.750</td>
      <td>29.890</td>
      <td>48.49000</td>
    </tr>
    <tr>
      <th>Orbital Period</th>
      <td>0.2410</td>
      <td>0.615</td>
      <td>1</td>
      <td>0.07480</td>
      <td>1.880</td>
      <td>11.900</td>
      <td>29.400</td>
      <td>83.700</td>
      <td>163.700</td>
      <td>247.90000</td>
    </tr>
    <tr>
      <th>Orbital Velocity</th>
      <td>1.5900</td>
      <td>1.180</td>
      <td>1</td>
      <td>0.03430</td>
      <td>0.808</td>
      <td>0.439</td>
      <td>0.325</td>
      <td>0.228</td>
      <td>0.182</td>
      <td>0.15700</td>
    </tr>
    <tr>
      <th>Orbital Eccentricity</th>
      <td>12.3000</td>
      <td>0.401</td>
      <td>1</td>
      <td>3.29000</td>
      <td>5.600</td>
      <td>2.930</td>
      <td>3.380</td>
      <td>2.740</td>
      <td>0.677</td>
      <td>14.60000</td>
    </tr>
    <tr>
      <th>Obliquity to Orbit</th>
      <td>0.0010</td>
      <td>0.113</td>
      <td>1</td>
      <td>0.28500</td>
      <td>1.070</td>
      <td>0.134</td>
      <td>1.140</td>
      <td>4.170</td>
      <td>1.210</td>
      <td>2.45000</td>
    </tr>
    <tr>
      <th>Surface Pressure</th>
      <td>0.0000</td>
      <td>92.000</td>
      <td>1</td>
      <td>0.00000</td>
      <td>0.010</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>0.00001</td>
    </tr>
    <tr>
      <th>Number of Moons</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>1</td>
      <td>0.00000</td>
      <td>2.000</td>
      <td>79.000</td>
      <td>82.000</td>
      <td>27.000</td>
      <td>14.000</td>
      <td>5.00000</td>
    </tr>
    <tr>
      <th>Ring System?</th>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0</td>
      <td>0.00000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>Global Magnetic Field?</th>
      <td>1.0000</td>
      <td>0.000</td>
      <td>1</td>
      <td>0.00000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>2.00000</td>
    </tr>
  </tbody>
</table>
</div>



But for purpose of analysis, it will better if the indices are planets, we can achieve this by taking the transpose of the table, where we interchange row and columns


```python
#Transpose index and columns.
#Reflect the DataFrame over its main diagonal by 
#writing rows as columns and vice-versa. 
#The property T is an accessor to the method transpose().
dfT = df.T

```


```python
#check if columns are changed
dfT.columns

```




    Index(['Mass', 'Diameter', 'Density', 'Gravity', 'Escape Velocity',
           'Rotation Period', 'Length of Day', 'Distance from Sun', 'Perihelion',
           'Aphelion', 'Orbital Period', 'Orbital Velocity',
           'Orbital Eccentricity', 'Obliquity to Orbit', 'Surface Pressure',
           'Number of Moons', 'Ring System?', 'Global Magnetic Field?'],
          dtype='object', name='Physical_Measurement')



Extract relevant columns for our analysis and then form a data frame, you can do so as follows


```python
#Extract relevant columns 
dfT = dfT[['Mass', 'Diameter', 'Density', 'Gravity', 'Escape Velocity',
       'Rotation Period', 'Length of Day', 'Distance from Sun', 'Perihelion',
       'Aphelion', 'Orbital Period', 'Orbital Velocity']]
dfT
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
      <th>Physical_Measurement</th>
      <th>Mass</th>
      <th>Diameter</th>
      <th>Density</th>
      <th>Gravity</th>
      <th>Escape Velocity</th>
      <th>Rotation Period</th>
      <th>Length of Day</th>
      <th>Distance from Sun</th>
      <th>Perihelion</th>
      <th>Aphelion</th>
      <th>Orbital Period</th>
      <th>Orbital Velocity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mercury</th>
      <td>0.0553</td>
      <td>0.3830</td>
      <td>0.984</td>
      <td>0.378</td>
      <td>0.384</td>
      <td>58.800</td>
      <td>175.900</td>
      <td>0.38700</td>
      <td>0.31300</td>
      <td>0.45900</td>
      <td>0.2410</td>
      <td>1.5900</td>
    </tr>
    <tr>
      <th>Venus</th>
      <td>0.8150</td>
      <td>0.9490</td>
      <td>0.951</td>
      <td>0.907</td>
      <td>0.926</td>
      <td>-244.000</td>
      <td>116.800</td>
      <td>0.72300</td>
      <td>0.73100</td>
      <td>0.71600</td>
      <td>0.6150</td>
      <td>1.1800</td>
    </tr>
    <tr>
      <th>Earth</th>
      <td>1.0000</td>
      <td>1.0000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>1.00000</td>
      <td>1.00000</td>
      <td>1.00000</td>
      <td>1.0000</td>
      <td>1.0000</td>
    </tr>
    <tr>
      <th>Moon</th>
      <td>0.0123</td>
      <td>0.2724</td>
      <td>0.605</td>
      <td>0.166</td>
      <td>0.213</td>
      <td>27.400</td>
      <td>29.500</td>
      <td>0.00257</td>
      <td>0.00247</td>
      <td>0.00267</td>
      <td>0.0748</td>
      <td>0.0343</td>
    </tr>
    <tr>
      <th>Mars</th>
      <td>0.1070</td>
      <td>0.5320</td>
      <td>0.713</td>
      <td>0.377</td>
      <td>0.450</td>
      <td>1.030</td>
      <td>1.030</td>
      <td>1.52000</td>
      <td>1.41000</td>
      <td>1.64000</td>
      <td>1.8800</td>
      <td>0.8080</td>
    </tr>
    <tr>
      <th>Jupiter</th>
      <td>317.8000</td>
      <td>11.2100</td>
      <td>0.240</td>
      <td>2.360</td>
      <td>5.320</td>
      <td>0.415</td>
      <td>0.414</td>
      <td>5.20000</td>
      <td>5.03000</td>
      <td>5.37000</td>
      <td>11.9000</td>
      <td>0.4390</td>
    </tr>
    <tr>
      <th>Saturn</th>
      <td>95.2000</td>
      <td>9.4500</td>
      <td>0.125</td>
      <td>0.916</td>
      <td>3.170</td>
      <td>0.445</td>
      <td>0.444</td>
      <td>9.58000</td>
      <td>9.20000</td>
      <td>9.96000</td>
      <td>29.4000</td>
      <td>0.3250</td>
    </tr>
    <tr>
      <th>Uranus</th>
      <td>14.5000</td>
      <td>4.0100</td>
      <td>0.230</td>
      <td>0.889</td>
      <td>1.900</td>
      <td>-0.720</td>
      <td>0.718</td>
      <td>19.20000</td>
      <td>18.64000</td>
      <td>19.75000</td>
      <td>83.7000</td>
      <td>0.2280</td>
    </tr>
    <tr>
      <th>Neptune</th>
      <td>17.1000</td>
      <td>3.8800</td>
      <td>0.297</td>
      <td>1.120</td>
      <td>2.100</td>
      <td>0.673</td>
      <td>0.671</td>
      <td>30.05000</td>
      <td>30.22000</td>
      <td>29.89000</td>
      <td>163.7000</td>
      <td>0.1820</td>
    </tr>
    <tr>
      <th>Pluto</th>
      <td>0.0025</td>
      <td>0.1860</td>
      <td>0.380</td>
      <td>0.071</td>
      <td>0.116</td>
      <td>6.410</td>
      <td>6.390</td>
      <td>39.48000</td>
      <td>30.16000</td>
      <td>48.49000</td>
      <td>247.9000</td>
      <td>0.1570</td>
    </tr>
  </tbody>
</table>
</div>



# Visualizations

Now everything is inorder and as we want it to be, it will be good idea to plot and visualize the parameters. Recall this is. Below we present plots of each columns versus planets, group of parameters versus planets, and stack all the columns in one plot.   

The eight (spoiler alert!) planets of our Solar System vary widely, not only in terms of size, but also in terms of other parameters. 

## Plots of individual column 

### Mass


```python
ax=dfT['Mass'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='lightgreen')
ax.set_xlabel("Planets")
ax.set_ylabel("Mass");
```


![png](/img/Scraping NASA for Escape Velocity/output_50_0.png)


We can see, as expected, Jupiter and Staurn are big. Because of a wide range of max and min values, values of 1 or below seems to be zero. Hard to see in this plpot, you can use ylim to need to see the results as follows 


```python
ax=dfT['Mass'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='lightgreen')
ax.set_xlabel("Planets")
#ax1.set_xlim([begin, end])
ax.set_ylim([0, 2])
ax.set_ylabel("Mass");
```


![png](/img/Scraping NASA for Escape Velocity/output_52_0.png)


### Escape Velocity


```python
ax=dfT['Escape Velocity'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='g')
ax.set_xlabel("Planets")
ax.set_ylabel("Escape Velocity");
```


![png](/img/Scraping NASA for Escape Velocity/output_54_0.png)


### Length of Day

**How old will you be on the other planets?**


```python
ax=dfT['Length of Day'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='m')
ax.set_xlabel("Planets")
ax.set_ylabel("Length of Day");
```


![png](/img/Scraping NASA for Escape Velocity/output_56_0.png)


You can find in [How Long Is One Day on other Planets](https://spaceplace.nasa.gov/days/en/) how long each is one day in each planet. The answer is
> **On Mercury a day lasts 1,408 hours, and on Venus it lasts 5,832 hours. On Earth and Mars it’s very similar. Earth takes 24 hours to complete one spin, and Mars takes 25 hours. The gas giants rotate really fast. Jupiter takes just 10 hours to complete one rotation. Saturn takes 11 hours, Uranus takes 17 hours, and Neptune takes 16 hours.**

You may notice that Mercury and Venus have a very long day. To make sense of it recall that a day is the amount of time it takes for a planet to completely spin around, its axis, and make one full rotation.

### Gravity


```python
ax=dfT['Gravity'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='r')
ax.set_xlabel("Planets")
ax.set_ylabel("Gravity");
```


![png](/img/Scraping NASA for Escape Velocity/output_59_0.png)


Gravity varies depending on the size, mass and density of the planet. In our solar system we can see that gravity varies, ranging from 0.38 g on Mercury and Mars to a powerful 2.528 g Jupiter's. And on the Moon, which is our natural satellite, it is a very small 0.1654 g.

### Density


```python
ax=dfT['Density'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='darkblue')
ax.set_xlabel("Planets")
ax.set_ylabel("Density");
```


![png](/img/Scraping NASA for Escape Velocity/output_62_0.png)



Density is defined as mass per unit of volume. We can see that the four inner planets – those that are closest to the Sun – are denser. This is because they are composed primarily of silicate rocks or metals and have a solid surface. 

The four outer planets are designated as gas giants this is so because they are composed primarily of of hydrogen, helium, and water existing in various physical states. Although size wise these planets are greater and heavier, their overall density is much lower. 

### Distance from the sun


```python
ax=dfT['Distance from Sun'].plot(kind='bar',x=dfT.index, figsize=(10,7),color='b')
ax.set_xlabel("Planets")
ax.set_ylabel("Distance from Sun ");
```


![png](/img/Scraping NASA for Escape Velocity/output_65_0.png)


## Plot of two or more columns

### Gravity, Density, Escape Velocity


```python
dfT1 = pd.DataFrame(dfT, columns=['Gravity', 'Density', 'Escape Velocity'])
colors=['red','blue','lightgreen']
ax=dfT1.plot.bar( figsize=(12,8), color=colors)
ax.set_xlabel("Planets")
ax.set_ylabel("Gravity, Density OR Escape Velocity");
```


![png](/img/Scraping NASA for Escape Velocity/output_67_0.png)


### Distance from the sun, Length of Day


```python
dfT2 = pd.DataFrame(dfT, columns=['Distance from Sun', 'Length of Day'])
ax=dfT2.plot.bar(figsize=(10,7))
ax.set_xlabel("Planets")
ax.set_ylabel("Distance from Sun OR Length of Day");
```


![png](/img/Scraping NASA for Escape Velocity/output_69_0.png)


## Stacked columns


```python
dfT3 = pd.DataFrame(dfT, columns=['Gravity', 'Density', 'Escape Velocity','Distance from Sun', 'Length of Day','Mass'])
ax=dfT3.plot.bar(stacked=True, figsize=(10,7),colormap='hsv',title='Defferent Physical Measurements of each Planet')
ax.set_xlabel("Planets")
ax.set_ylabel("Physical Parameters");
```


![png](/img/Scraping NASA for Escape Velocity/output_71_0.png)


## Scaled Stacked Columns


```python

scaler = MinMaxScaler()
dfT4 = pd.DataFrame(scaler.fit_transform(dfT3), columns=dfT3.columns)
```


```python
ax=dfT4.plot.bar(stacked=True, figsize=(10,7),colormap='rainbow',title='Defferent Physical Measurements of each Planet')
ax.set_xlabel("Planets")
ax.set_ylabel("Physical Parameters");
```


![png](/img/Scraping NASA for Escape Velocity/output_74_0.png)

