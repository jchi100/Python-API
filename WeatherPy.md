

```python
from citipy import citipy
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import json
from pprint import pprint
import seaborn as sn
from datetime import datetime
```


```python

x, y = np.random.uniform(-180,180,size=600), np.random.uniform(-90, 90, size=600)

points = list(zip(x,y))

```


```python
plt.figure(figsize=(10,6))
plt.scatter(x,y)

plt.title("City Distribution")
plt.xlabel('Latitude')
plt.ylabel('Longitude')

plt.show()
```


![png](output_2_0.png)



```python
city_df = pd.DataFrame(points)

city_df= city_df.rename(columns={0:'Lat',1:'Lng'})
city_list = []
country_list = []


for i in range(0,len(city_df['Lat'])):
    x = city_df['Lat'][i]
    y = city_df['Lng'][i]

    city = citipy.nearest_city(x, y)
    city_list.append(city.city_name)
    country_list.append(city.country_code.upper())
    
city_df['City'] = city_list
city_df['Country'] = country_list
city_df.head()
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
      <th>Lat</th>
      <th>Lng</th>
      <th>City</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-38.835650</td>
      <td>46.791351</td>
      <td>tsihombe</td>
      <td>MG</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-116.221667</td>
      <td>-1.636815</td>
      <td>hermanus</td>
      <td>ZA</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-116.736019</td>
      <td>-28.798833</td>
      <td>ushuaia</td>
      <td>AR</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-171.701913</td>
      <td>-44.702279</td>
      <td>ushuaia</td>
      <td>AR</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-54.753320</td>
      <td>-7.591619</td>
      <td>cape town</td>
      <td>ZA</td>
    </tr>
  </tbody>
</table>
</div>




```python
url = "http://api.openweathermap.org/data/2.5/weather?units=Imperial&appid="
api_key = "18c63e24eb1fec74900cd4a019a75768"

```


```python
cloud_list = []
date_list = [] 
humidity_list = []
maxtemp_list = []
windspeed_list = []

with open('WeatherPyLog.txt','w') as output:
    output.write("WeatherPy Log: \n\r")
    
    for index, city in enumerate(city_list):
        
        
        str = "&q=" + city
       
        response = requests.get(url + api_key + str)
        output.write("URL: " + url + api_key + str+"\n\r")
        output.write("Retrieving Record: " + "{}".format(index+1) + ": "+city +" data -----------------\n\r")
        
        if response.ok:
            weatherdata = response.json()
        
           # date_list.append(weatherdata['dt'])
            currentdate = datetime.fromtimestamp(weatherdata['dt']).strftime('%m/%d/%Y')
            date_list.append(currentdate)
            cloud_list.append(weatherdata['clouds']['all'])
            humidity_list.append(weatherdata['main']['humidity'])
            maxtemp_list.append(weatherdata['main']['temp_max'])
            windspeed_list.append(weatherdata['wind']['speed'])
            5           
        else:            
            date_list.append(np.nan)
            cloud_list.append(np.nan)
            humidity_list.append(np.nan)
            maxtemp_list.append(np.nan)
            windspeed_list.append(np.nan)
            
            output.write(city +" data not found-----------------\n\r")

city_df['Date'] = date_list
city_df['Cloudiness'] = cloud_list
city_df['Humidity'] = humidity_list
city_df['Max Temp'] = maxtemp_list
city_df['Wind Speed'] = windspeed_list

city_df.head()
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
      <th>Lat</th>
      <th>Lng</th>
      <th>City</th>
      <th>Country</th>
      <th>Date</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-38.835650</td>
      <td>46.791351</td>
      <td>tsihombe</td>
      <td>MG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-116.221667</td>
      <td>-1.636815</td>
      <td>hermanus</td>
      <td>ZA</td>
      <td>03/19/2018</td>
      <td>0.0</td>
      <td>99.0</td>
      <td>48.5</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-116.736019</td>
      <td>-28.798833</td>
      <td>ushuaia</td>
      <td>AR</td>
      <td>03/19/2018</td>
      <td>75.0</td>
      <td>86.0</td>
      <td>41.0</td>
      <td>33.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-171.701913</td>
      <td>-44.702279</td>
      <td>ushuaia</td>
      <td>AR</td>
      <td>03/19/2018</td>
      <td>75.0</td>
      <td>86.0</td>
      <td>41.0</td>
      <td>33.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-54.753320</td>
      <td>-7.591619</td>
      <td>cape town</td>
      <td>ZA</td>
      <td>03/19/2018</td>
      <td>0.0</td>
      <td>82.0</td>
      <td>60.8</td>
      <td>11.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_df = city_df.dropna(how='any')

city_df = city_df.head(500)
city_df.head()
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
      <th>Lat</th>
      <th>Lng</th>
      <th>City</th>
      <th>Country</th>
      <th>Date</th>
      <th>Cloudiness</th>
      <th>Humidity</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>-116.221667</td>
      <td>-1.636815</td>
      <td>hermanus</td>
      <td>ZA</td>
      <td>03/19/2018</td>
      <td>0.0</td>
      <td>99.0</td>
      <td>48.50</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-116.736019</td>
      <td>-28.798833</td>
      <td>ushuaia</td>
      <td>AR</td>
      <td>03/19/2018</td>
      <td>75.0</td>
      <td>86.0</td>
      <td>41.00</td>
      <td>33.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-171.701913</td>
      <td>-44.702279</td>
      <td>ushuaia</td>
      <td>AR</td>
      <td>03/19/2018</td>
      <td>75.0</td>
      <td>86.0</td>
      <td>41.00</td>
      <td>33.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-54.753320</td>
      <td>-7.591619</td>
      <td>cape town</td>
      <td>ZA</td>
      <td>03/19/2018</td>
      <td>0.0</td>
      <td>82.0</td>
      <td>60.80</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>5</th>
      <td>147.112033</td>
      <td>-68.319219</td>
      <td>qaanaaq</td>
      <td>GL</td>
      <td>03/19/2018</td>
      <td>0.0</td>
      <td>98.0</td>
      <td>-17.21</td>
      <td>11.23</td>
    </tr>
  </tbody>
</table>
</div>




```python

plt.figure(figsize=(10,6))
plt.scatter(city_df['Lat'], city_df['Max Temp'],color = 'DarkBlue',edgecolor='Black',alpha=0.5)
sn.set()

plt.title("City Latitude vs. Max Temperature ("+city_df.iloc[0]['Date']+")")
plt.xlabel('Latitude')
plt.ylabel('Max Temperature (F)')

plt.savefig('CityLat_MaxTemperature.png')
plt.show()
```


![png](output_7_0.png)



```python
plt.figure(figsize=(10,6))
plt.scatter(city_df['Lat'], city_df['Humidity'],color = 'DarkBlue',edgecolor='Black',alpha=0.5)
sn.set()

plt.title("City Latitude vs. Humidity "+city_df.iloc[0]['Date'])
plt.xlabel('Latitude')
plt.ylabel('Humidity (%)')

plt.savefig('CityLat_Humidity.png')
plt.show()
```


![png](output_8_0.png)



```python
plt.figure(figsize=(10,6))
plt.scatter(city_df['Lat'], city_df['Cloudiness'],color = 'DarkBlue',edgecolor='Black',alpha=0.5)
sn.set()

plt.title("City Latitude vs. Cloudiness ("+city_df.iloc[0]['Date'] +")")
plt.xlabel('Latitude')
plt.ylabel('Cloudiness (%)')

plt.savefig('CityLat_Cloudiness.png')
plt.show()
```


![png](output_9_0.png)



```python
plt.figure(figsize=(10,6))
plt.scatter(city_df['Lat'], city_df['Wind Speed'],color = 'DarkBlue',edgecolor='Black',alpha=0.5)
sn.set()

plt.title("City Latitude vs. Wind Speed ("+city_df.iloc[0]['Date']+")")
plt.xlabel('Latitude')
plt.ylabel('Wind Speed (mph)')

plt.savefig('CityLat_WindSpeed.png')
plt.show()
```


![png](output_10_0.png)



```python
city_df.to_csv("WeatherPyOutput.csv", sep=',', encoding='utf-8')
```
