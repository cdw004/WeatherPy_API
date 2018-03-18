
Objectives: 

Your objective is to build a series of scatter plots to showcase the following relationships:

Temperature (F) vs. Latitude
Humidity (%) vs. Latitude
Cloudiness (%) vs. Latitude
Wind Speed (mph) vs. Latitude

Your final notebook must:

Randomly select at least 500 unique (non-repeat) cities based on latitude and longitude.
Perform a weather check on each of the cities using a series of successive API calls.
Include a print log of each city as it's being processed with the city number, city name, and requested URL.
Save both a CSV of all data retrieved and png images for each scatter plot.


As final considerations:

You must use the Matplotlib and Seaborn libraries.
You must include a written description of three observable trends based on the data.
You must use proper labeling of your plots, including aspects like: Plot Titles (with date of analysis) and Axes Labels.
You must include an exported markdown version of your Notebook called  README.md in your GitHub repository.
See Example Solution for a reference on expected format.


```python
#dependencies
import os
import json
from pprint import pprint
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from citipy import citipy
import random
from datetime import datetime
import requests
from random import uniform
#API keys
#OpenWeather API Key
from config import api_key1
#Google API Key
from config import api_key2
```


```python
#Method 1 - randomly generating coordinates, however I only receive 1 city, seek alternative methods.
#rand_city = []
#while len(rand_city) <= 500:
#for x in range (0,500):
#    for y in range(0,500):
#        x,y = uniform(-180,180), uniform(-90, 90)
        
#rand_city_name = citipy.nearest_city(x,y).city_name

#print(rand_city_name)
#rand_city.append(random_city_name)
#rand_city
```


```python
#Method 2 - develop pd data frame

#500 random cities

city = []
country = []
latitude=[]
longitude = []


location_data = pd.DataFrame()

#generate coordinates
location_data['latitude'] = [np.random.uniform(-90,90) for x in range(1500)]
location_data['longitude'] = [np.random.uniform(-180, 180) for x in range(1500)]

#develop columns & spot check
location_data[city] = ''
location_data[country] = ''
location_data[latitude] = ''
location_data[longitude] = ''

#location_data

#Add City & Country

for index, row in location_data.iterrows():
    latitude = row['latitude']
    longitude = row['longitude']
    location_data.set_value(index, 'city', citipy.nearest_city(latitude, longitude).city_name)
    location_data.set_value(index, 'country', citipy.nearest_city(latitude, longitude).country_code)

#require unique values so check for duplicates
#drop_duplicates - https://stackoverflow.com/questions/23667369/drop-all-duplicate-rows-in-python-pandas
location_data = location_data.drop_duplicates(['city', 'country'])
location_data = location_data.dropna()    

print(len(location_data['city'].value_counts()))
location_data.head()
```

    /Users/cdw004/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel/__main__.py:30: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/cdw004/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel/__main__.py:31: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead


    605





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
      <th>latitude</th>
      <th>longitude</th>
      <th>city</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.179579</td>
      <td>52.596091</td>
      <td>abu dhabi</td>
      <td>ae</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17.783145</td>
      <td>-132.161067</td>
      <td>lompoc</td>
      <td>us</td>
    </tr>
    <tr>
      <th>2</th>
      <td>48.280493</td>
      <td>49.739156</td>
      <td>inderborskiy</td>
      <td>kz</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-71.939904</td>
      <td>-20.524464</td>
      <td>mar del plata</td>
      <td>ar</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.468411</td>
      <td>39.259181</td>
      <td>tawkar</td>
      <td>sd</td>
    </tr>
  </tbody>
</table>
</div>




```python
city.append(location_data['city'])
country.append(location_data['country'])
print(city)
```

    [0                       abu dhabi
    1                          lompoc
    2                    inderborskiy
    3                   mar del plata
    4                          tawkar
    5                         rikitea
    6                     morgan city
    7                           vaini
    9                      port blair
    10                   punta arenas
    11                        ushuaia
    12                        mataura
    13               illoqqortoormiut
    15                     bredasdorp
    17                          bluff
    18                       shintomi
    20         santa maria da vitoria
    21                        qaanaaq
    22                      jamestown
    23                    port alfred
    26                         albany
    27                      cape town
    28                 ribeira grande
    29                       solbjerg
    30                          ancud
    31                          ikeda
    32                       matagami
    35                       hermanus
    36                      dongsheng
    37                           jalu
                      ...            
    1403                   oranjemund
    1405                      leiyang
    1408                 richards bay
    1414                      cascais
    1425                      aljezur
    1429                         gizo
    1430                   phalaborwa
    1432    petropavlovsk-kamchatskiy
    1433                    egvekinot
    1434                       boueni
    1436                     tuskegee
    1438                     udachnyy
    1439                     breytovo
    1443                        avera
    1448                   bar harbor
    1449                 port lincoln
    1456                         alta
    1458                   skibbereen
    1460                      maralal
    1473                      salalah
    1474                     sobolevo
    1475                       kokoda
    1482                     dingzhou
    1483                       igunga
    1485                 batemans bay
    1486                   port hardy
    1490                        dunda
    1491                       marawi
    1492                       warqla
    1494                      bakchar
    Name: city, Length: 609, dtype: object]



```python
url = "http://api.openweathermap.org/data/2.5/weather?"

apikey = api_key1
units = "Imperial"
count = 0

location_data['temperature'] = ""
location_data['humidity'] = ""
location_data['cloudiness'] = ""
location_data['wind_speed'] = ""

for index,row in location_data.iterrows():
    count+= 1
    query_url = url + "appid=" + apikey + "&units=" + units + "&q=" + row['city']

    try:
        weather_response = requests.get(query_url)
        weather = weather_response.json()
#         print(cityweather)

        location_data.set_value(index, "temperature", int(weather['main']['temp']))
        location_data.set_value(index, "humidity", int(weather['main']['humidity']))
        location_data.set_value(index, "cloudiness", int(weather['clouds']['all']))
        location_data.set_value(index, "wind_speed", int(weather['wind']['speed']))
    except:
        print("No data for this city: {row['city']}")
        location_data.drop([index])
        
    print(f"City#: {count}")
    print(f"City Name: {row['city']}" )
    print(f"URL: {query_url}")
```

    /Users/cdw004/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel/__main__.py:21: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/cdw004/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel/__main__.py:22: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/cdw004/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel/__main__.py:23: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
    /Users/cdw004/anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel/__main__.py:24: FutureWarning: set_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead


    City#: 1
    City Name: abu dhabi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=abu dhabi
    City#: 2
    City Name: lompoc
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lompoc
    No data for this city: {row['city']}
    City#: 3
    City Name: inderborskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=inderborskiy
    City#: 4
    City Name: mar del plata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mar del plata
    No data for this city: {row['city']}
    City#: 5
    City Name: tawkar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tawkar
    City#: 6
    City Name: rikitea
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=rikitea
    City#: 7
    City Name: morgan city
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=morgan city
    City#: 8
    City Name: vaini
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=vaini
    City#: 9
    City Name: port blair
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=port blair
    City#: 10
    City Name: punta arenas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=punta arenas
    City#: 11
    City Name: ushuaia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ushuaia
    City#: 12
    City Name: mataura
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mataura
    No data for this city: {row['city']}
    City#: 13
    City Name: illoqqortoormiut
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=illoqqortoormiut
    City#: 14
    City Name: bredasdorp
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bredasdorp
    City#: 15
    City Name: bluff
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bluff
    City#: 16
    City Name: shintomi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=shintomi
    City#: 17
    City Name: santa maria da vitoria
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=santa maria da vitoria
    City#: 18
    City Name: qaanaaq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=qaanaaq
    City#: 19
    City Name: jamestown
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jamestown
    City#: 20
    City Name: port alfred
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=port alfred
    City#: 21
    City Name: albany
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=albany
    City#: 22
    City Name: cape town
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cape town
    City#: 23
    City Name: ribeira grande
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ribeira grande
    City#: 24
    City Name: solbjerg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=solbjerg
    City#: 25
    City Name: ancud
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ancud
    City#: 26
    City Name: ikeda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ikeda
    City#: 27
    City Name: matagami
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=matagami
    City#: 28
    City Name: hermanus
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hermanus
    City#: 29
    City Name: dongsheng
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dongsheng
    City#: 30
    City Name: jalu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jalu
    City#: 31
    City Name: nikolskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nikolskoye
    City#: 32
    City Name: pevek
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pevek
    City#: 33
    City Name: shache
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=shache
    City#: 34
    City Name: busselton
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=busselton
    City#: 35
    City Name: broken hill
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=broken hill
    City#: 36
    City Name: omsukchan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=omsukchan
    City#: 37
    City Name: san juan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san juan
    City#: 38
    City Name: hilo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hilo
    No data for this city: {row['city']}
    City#: 39
    City Name: tumannyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tumannyy
    City#: 40
    City Name: buala
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=buala
    City#: 41
    City Name: tiksi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tiksi
    City#: 42
    City Name: sao joao da barra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sao joao da barra
    City#: 43
    City Name: ambilobe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ambilobe
    City#: 44
    City Name: flinders
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=flinders
    City#: 45
    City Name: laguna de perlas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=laguna de perlas
    City#: 46
    City Name: guerrero negro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=guerrero negro
    City#: 47
    City Name: ponta do sol
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ponta do sol
    City#: 48
    City Name: ubinskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ubinskoye
    No data for this city: {row['city']}
    City#: 49
    City Name: belushya guba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=belushya guba
    City#: 50
    City Name: torbay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=torbay
    City#: 51
    City Name: manmad
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=manmad
    City#: 52
    City Name: clyde river
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=clyde river
    City#: 53
    City Name: atuona
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=atuona
    City#: 54
    City Name: arica
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=arica
    City#: 55
    City Name: dzilam gonzalez
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dzilam gonzalez
    City#: 56
    City Name: east london
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=east london
    City#: 57
    City Name: pacifica
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pacifica
    City#: 58
    City Name: barentu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=barentu
    City#: 59
    City Name: conde
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=conde
    No data for this city: {row['city']}
    City#: 60
    City Name: taolanaro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=taolanaro
    City#: 61
    City Name: safford
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=safford
    City#: 62
    City Name: norman wells
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=norman wells
    City#: 63
    City Name: namibe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=namibe
    City#: 64
    City Name: murovane
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=murovane
    City#: 65
    City Name: aquiraz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aquiraz
    City#: 66
    City Name: ixtapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ixtapa
    City#: 67
    City Name: avarua
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=avarua
    City#: 68
    City Name: caravelas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=caravelas
    City#: 69
    City Name: jodhpur
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jodhpur
    City#: 70
    City Name: itabera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=itabera
    City#: 71
    City Name: groa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=groa
    No data for this city: {row['city']}
    City#: 72
    City Name: bolungarvik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bolungarvik
    City#: 73
    City Name: bethel
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bethel
    City#: 74
    City Name: soyo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=soyo
    City#: 75
    City Name: ketchikan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ketchikan
    City#: 76
    City Name: mandera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mandera
    City#: 77
    City Name: gigmoto
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gigmoto
    City#: 78
    City Name: zeya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zeya
    No data for this city: {row['city']}
    City#: 79
    City Name: fort saint john
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=fort saint john
    No data for this city: {row['city']}
    City#: 80
    City Name: barentsburg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=barentsburg
    City#: 81
    City Name: kapaa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kapaa
    City#: 82
    City Name: hwange
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hwange
    No data for this city: {row['city']}
    City#: 83
    City Name: andarab
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=andarab
    City#: 84
    City Name: soure
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=soure
    City#: 85
    City Name: roebourne
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=roebourne
    City#: 86
    City Name: igarka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=igarka
    City#: 87
    City Name: la asuncion
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=la asuncion
    No data for this city: {row['city']}
    City#: 88
    City Name: attawapiskat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=attawapiskat
    City#: 89
    City Name: tekirdag
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tekirdag
    City#: 90
    City Name: mae sai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mae sai
    City#: 91
    City Name: puerto ayora
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=puerto ayora
    City#: 92
    City Name: victoria
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=victoria
    City#: 93
    City Name: thompson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=thompson
    City#: 94
    City Name: butaritari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=butaritari
    City#: 95
    City Name: talnakh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=talnakh
    City#: 96
    City Name: new norfolk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=new norfolk
    City#: 97
    City Name: tuktoyaktuk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tuktoyaktuk
    City#: 98
    City Name: hobart
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hobart
    City#: 99
    City Name: iqaluit
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=iqaluit
    City#: 100
    City Name: palauig
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=palauig
    City#: 101
    City Name: butajira
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=butajira
    City#: 102
    City Name: padang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=padang
    No data for this city: {row['city']}
    City#: 103
    City Name: meyungs
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=meyungs
    No data for this city: {row['city']}
    City#: 104
    City Name: bolshoy tsaryn
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bolshoy tsaryn
    No data for this city: {row['city']}
    City#: 105
    City Name: sentyabrskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sentyabrskiy
    City#: 106
    City Name: capreol
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=capreol
    No data for this city: {row['city']}
    City#: 107
    City Name: mys shmidta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mys shmidta
    No data for this city: {row['city']}
    City#: 108
    City Name: samusu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=samusu
    City#: 109
    City Name: kodiak
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kodiak
    City#: 110
    City Name: tessalit
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tessalit
    City#: 111
    City Name: hithadhoo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hithadhoo
    City#: 112
    City Name: khadyzhensk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=khadyzhensk
    City#: 113
    City Name: ahipara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ahipara
    City#: 114
    City Name: tasiilaq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tasiilaq
    No data for this city: {row['city']}
    City#: 115
    City Name: haibowan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=haibowan
    City#: 116
    City Name: cravo norte
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cravo norte
    City#: 117
    City Name: northam
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=northam
    City#: 118
    City Name: meulaboh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=meulaboh
    City#: 119
    City Name: gorontalo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gorontalo
    City#: 120
    City Name: saldanha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saldanha
    City#: 121
    City Name: kununurra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kununurra
    City#: 122
    City Name: hambantota
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hambantota
    City#: 123
    City Name: nanortalik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nanortalik
    City#: 124
    City Name: kichera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kichera
    City#: 125
    City Name: akdepe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=akdepe
    City#: 126
    City Name: kangaba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kangaba
    City#: 127
    City Name: tiznit
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tiznit
    City#: 128
    City Name: ahuimanu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ahuimanu
    City#: 129
    City Name: barrow
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=barrow
    City#: 130
    City Name: tromso
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tromso
    City#: 131
    City Name: hami
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hami
    City#: 132
    City Name: constitucion
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=constitucion
    No data for this city: {row['city']}
    City#: 133
    City Name: korla
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=korla
    City#: 134
    City Name: lebu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lebu
    City#: 135
    City Name: nuuk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nuuk
    City#: 136
    City Name: esperance
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=esperance
    City#: 137
    City Name: georgetown
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=georgetown
    No data for this city: {row['city']}
    City#: 138
    City Name: kyra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kyra
    City#: 139
    City Name: arraial do cabo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=arraial do cabo
    City#: 140
    City Name: bubaque
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bubaque
    City#: 141
    City Name: amapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=amapa
    City#: 142
    City Name: vostok
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=vostok
    City#: 143
    City Name: albanel
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=albanel
    City#: 144
    City Name: saint george
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint george
    City#: 145
    City Name: kruisfontein
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kruisfontein
    City#: 146
    City Name: faanui
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=faanui
    City#: 147
    City Name: kaitangata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kaitangata
    City#: 148
    City Name: khatanga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=khatanga
    No data for this city: {row['city']}
    City#: 149
    City Name: lolua
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lolua
    City#: 150
    City Name: muros
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=muros
    City#: 151
    City Name: kota tinggi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kota tinggi
    City#: 152
    City Name: chabahar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chabahar
    City#: 153
    City Name: vernon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=vernon
    City#: 154
    City Name: norton shores
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=norton shores
    City#: 155
    City Name: longyearbyen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=longyearbyen
    City#: 156
    City Name: chino valley
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chino valley
    City#: 157
    City Name: sorland
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sorland
    City#: 158
    City Name: chuy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chuy
    City#: 159
    City Name: kahului
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kahului
    No data for this city: {row['city']}
    City#: 160
    City Name: stoyba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=stoyba
    City#: 161
    City Name: moron
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=moron
    City#: 162
    City Name: ollioules
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ollioules
    City#: 163
    City Name: upernavik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=upernavik
    City#: 164
    City Name: lorengau
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lorengau
    No data for this city: {row['city']}
    City#: 165
    City Name: barbar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=barbar
    City#: 166
    City Name: zhigansk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zhigansk
    City#: 167
    City Name: changuinola
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=changuinola
    No data for this city: {row['city']}
    City#: 168
    City Name: grand river south east
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=grand river south east
    No data for this city: {row['city']}
    City#: 169
    City Name: imisli
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=imisli
    City#: 170
    City Name: winnemucca
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=winnemucca
    City#: 171
    City Name: baherden
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=baherden
    City#: 172
    City Name: malinyi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=malinyi
    City#: 173
    City Name: kungurtug
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kungurtug
    City#: 174
    City Name: saint andrews
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint andrews
    City#: 175
    City Name: provideniya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=provideniya
    City#: 176
    City Name: hobyo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hobyo
    City#: 177
    City Name: severo-kurilsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=severo-kurilsk
    No data for this city: {row['city']}
    City#: 178
    City Name: ndele
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ndele
    City#: 179
    City Name: san roque
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san roque
    City#: 180
    City Name: belyy yar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=belyy yar
    City#: 181
    City Name: olinda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=olinda
    City#: 182
    City Name: hualmay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hualmay
    City#: 183
    City Name: rosarito
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=rosarito
    City#: 184
    City Name: plouzane
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=plouzane
    City#: 185
    City Name: cherskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cherskiy
    City#: 186
    City Name: christchurch
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=christchurch
    City#: 187
    City Name: sarana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sarana
    City#: 188
    City Name: bolobo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bolobo
    City#: 189
    City Name: aromashevo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aromashevo
    City#: 190
    City Name: tahoua
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tahoua
    City#: 191
    City Name: hamilton
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hamilton
    City#: 192
    City Name: yellowknife
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=yellowknife
    City#: 193
    City Name: khandyga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=khandyga
    No data for this city: {row['city']}
    City#: 194
    City Name: amderma
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=amderma
    City#: 195
    City Name: inuvik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=inuvik
    City#: 196
    City Name: kieta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kieta
    City#: 197
    City Name: nancha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nancha
    City#: 198
    City Name: launceston
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=launceston
    No data for this city: {row['city']}
    City#: 199
    City Name: bengkulu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bengkulu
    City#: 200
    City Name: artemisa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=artemisa
    City#: 201
    City Name: nampula
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nampula
    City#: 202
    City Name: juanjui
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=juanjui
    City#: 203
    City Name: darnah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=darnah
    City#: 204
    City Name: kloulklubed
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kloulklubed
    City#: 205
    City Name: afua
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=afua
    City#: 206
    City Name: castro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=castro
    No data for this city: {row['city']}
    City#: 207
    City Name: cheuskiny
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cheuskiny
    City#: 208
    City Name: gubkinskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gubkinskiy
    No data for this city: {row['city']}
    City#: 209
    City Name: saryshagan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saryshagan
    City#: 210
    City Name: la ronge
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=la ronge
    City#: 211
    City Name: pisco
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pisco
    City#: 212
    City Name: ocampo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ocampo
    City#: 213
    City Name: filadelfia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=filadelfia
    City#: 214
    City Name: boende
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=boende
    City#: 215
    City Name: saint-nazaire
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint-nazaire
    No data for this city: {row['city']}
    City#: 216
    City Name: svetlyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=svetlyy
    City#: 217
    City Name: amga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=amga
    City#: 218
    City Name: cabo san lucas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cabo san lucas
    City#: 219
    City Name: kysyl-syr
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kysyl-syr
    City#: 220
    City Name: aitape
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aitape
    City#: 221
    City Name: saint-francois
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint-francois
    City#: 222
    City Name: poum
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=poum
    City#: 223
    City Name: kutum
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kutum
    City#: 224
    City Name: puerto escondido
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=puerto escondido
    City#: 225
    City Name: yenagoa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=yenagoa
    City#: 226
    City Name: katsuura
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=katsuura
    City#: 227
    City Name: ko samui
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ko samui
    City#: 228
    City Name: college
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=college
    City#: 229
    City Name: mount gambier
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mount gambier
    City#: 230
    City Name: airai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=airai
    City#: 231
    City Name: saint anthony
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint anthony
    City#: 232
    City Name: kropotkin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kropotkin
    City#: 233
    City Name: dikson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dikson
    City#: 234
    City Name: carnarvon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=carnarvon
    City#: 235
    City Name: la serena
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=la serena
    City#: 236
    City Name: marienburg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=marienburg
    No data for this city: {row['city']}
    City#: 237
    City Name: san bartolome de tirajana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san bartolome de tirajana
    City#: 238
    City Name: zadar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zadar
    No data for this city: {row['city']}
    City#: 239
    City Name: saleaula
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saleaula
    City#: 240
    City Name: durango
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=durango
    City#: 241
    City Name: qasigiannguit
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=qasigiannguit
    City#: 242
    City Name: sayyan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sayyan
    City#: 243
    City Name: aklavik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aklavik
    City#: 244
    City Name: mackay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mackay
    No data for this city: {row['city']}
    City#: 245
    City Name: jambol
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jambol
    City#: 246
    City Name: saint-philippe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint-philippe
    City#: 247
    City Name: nkowakowa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nkowakowa
    City#: 248
    City Name: kondinskoye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kondinskoye
    City#: 249
    City Name: sao filipe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sao filipe
    No data for this city: {row['city']}
    City#: 250
    City Name: tabiauea
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tabiauea
    City#: 251
    City Name: slyudyanka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=slyudyanka
    City#: 252
    City Name: deputatskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=deputatskiy
    City#: 253
    City Name: moerai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=moerai
    City#: 254
    City Name: amalapuram
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=amalapuram
    City#: 255
    City Name: sundargarh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sundargarh
    City#: 256
    City Name: ostroleka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ostroleka
    City#: 257
    City Name: taoudenni
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=taoudenni
    City#: 258
    City Name: labuhan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=labuhan
    City#: 259
    City Name: luangwa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=luangwa
    City#: 260
    City Name: nemuro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nemuro
    City#: 261
    City Name: tolaga bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tolaga bay
    City#: 262
    City Name: concepcion
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=concepcion
    City#: 263
    City Name: alice springs
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=alice springs
    City#: 264
    City Name: jacareacanga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jacareacanga
    City#: 265
    City Name: kavieng
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kavieng
    City#: 266
    City Name: hasaki
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hasaki
    City#: 267
    City Name: kupang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kupang
    No data for this city: {row['city']}
    City#: 268
    City Name: cozumel
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cozumel
    City#: 269
    City Name: tuatapere
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tuatapere
    City#: 270
    City Name: port elizabeth
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=port elizabeth
    No data for this city: {row['city']}
    City#: 271
    City Name: bac lieu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bac lieu
    City#: 272
    City Name: chokurdakh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chokurdakh
    City#: 273
    City Name: araouane
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=araouane
    No data for this city: {row['city']}
    City#: 274
    City Name: kuah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kuah
    City#: 275
    City Name: catuday
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=catuday
    City#: 276
    City Name: sedkyrkeshch
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sedkyrkeshch
    City#: 277
    City Name: grand-lahou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=grand-lahou
    City#: 278
    City Name: lexington park
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lexington park
    City#: 279
    City Name: bambanglipuro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bambanglipuro
    City#: 280
    City Name: havre-saint-pierre
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=havre-saint-pierre
    City#: 281
    City Name: ponta do sol
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ponta do sol
    City#: 282
    City Name: sitka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sitka
    No data for this city: {row['city']}
    City#: 283
    City Name: zhanatas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zhanatas
    City#: 284
    City Name: bonavista
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bonavista
    City#: 285
    City Name: luena
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=luena
    City#: 286
    City Name: coihaique
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=coihaique
    City#: 287
    City Name: pirai do sul
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pirai do sul
    City#: 288
    City Name: bambous virieux
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bambous virieux
    City#: 289
    City Name: haines junction
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=haines junction
    City#: 290
    City Name: paamiut
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=paamiut
    City#: 291
    City Name: taltal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=taltal
    No data for this city: {row['city']}
    City#: 292
    City Name: tambul
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tambul
    City#: 293
    City Name: korem
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=korem
    No data for this city: {row['city']}
    City#: 294
    City Name: borama
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=borama
    City#: 295
    City Name: dingle
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dingle
    City#: 296
    City Name: mount isa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mount isa
    City#: 297
    City Name: mushie
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mushie
    City#: 298
    City Name: acajutla
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=acajutla
    City#: 299
    City Name: fort-shevchenko
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=fort-shevchenko
    No data for this city: {row['city']}
    City#: 300
    City Name: kadykchan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kadykchan
    City#: 301
    City Name: moorhead
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=moorhead
    City#: 302
    City Name: mnogovershinnyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mnogovershinnyy
    City#: 303
    City Name: strezhevoy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=strezhevoy
    City#: 304
    City Name: pacific grove
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pacific grove
    City#: 305
    City Name: grindavik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=grindavik
    City#: 306
    City Name: nouadhibou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nouadhibou
    City#: 307
    City Name: teknaf
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=teknaf
    City#: 308
    City Name: hervey bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hervey bay
    City#: 309
    City Name: saskylakh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saskylakh
    City#: 310
    City Name: north platte
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=north platte
    City#: 311
    City Name: mahebourg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mahebourg
    No data for this city: {row['city']}
    City#: 312
    City Name: rungata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=rungata
    City#: 313
    City Name: coquimbo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=coquimbo
    City#: 314
    City Name: kiruna
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kiruna
    City#: 315
    City Name: humberto de campos
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=humberto de campos
    City#: 316
    City Name: tocopilla
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tocopilla
    City#: 317
    City Name: cidreira
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cidreira
    City#: 318
    City Name: tahe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tahe
    City#: 319
    City Name: saalfeld
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saalfeld
    City#: 320
    City Name: trairi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=trairi
    No data for this city: {row['city']}
    City#: 321
    City Name: dinsor
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dinsor
    No data for this city: {row['city']}
    City#: 322
    City Name: tsienyane
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tsienyane
    City#: 323
    City Name: tommot
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tommot
    City#: 324
    City Name: ngunguru
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ngunguru
    City#: 325
    City Name: ouallam
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ouallam
    City#: 326
    City Name: anjangaon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=anjangaon
    City#: 327
    City Name: geraldton
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=geraldton
    City#: 328
    City Name: evensk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=evensk
    City#: 329
    City Name: anadyr
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=anadyr
    City#: 330
    City Name: sioux lookout
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sioux lookout
    City#: 331
    City Name: jijiga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jijiga
    No data for this city: {row['city']}
    City#: 332
    City Name: umzimvubu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=umzimvubu
    City#: 333
    City Name: leningradskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=leningradskiy
    No data for this city: {row['city']}
    City#: 334
    City Name: krasnoselkup
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=krasnoselkup
    City#: 335
    City Name: saint-augustin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint-augustin
    City#: 336
    City Name: luderitz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=luderitz
    City#: 337
    City Name: jadu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jadu
    City#: 338
    City Name: gasa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gasa
    City#: 339
    City Name: lavrentiya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lavrentiya
    City#: 340
    City Name: atlantis
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=atlantis
    City#: 341
    City Name: graham
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=graham
    City#: 342
    City Name: rawson
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=rawson
    City#: 343
    City Name: codrington
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=codrington
    City#: 344
    City Name: locri
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=locri
    City#: 345
    City Name: havoysund
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=havoysund
    City#: 346
    City Name: bilibino
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bilibino
    City#: 347
    City Name: thai binh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=thai binh
    City#: 348
    City Name: arlit
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=arlit
    City#: 349
    City Name: worthington
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=worthington
    City#: 350
    City Name: aketi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aketi
    City#: 351
    City Name: ayolas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ayolas
    City#: 352
    City Name: san policarpo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san policarpo
    City#: 353
    City Name: itaituba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=itaituba
    City#: 354
    City Name: muhororo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=muhororo
    City#: 355
    City Name: ninh binh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ninh binh
    City#: 356
    City Name: pitimbu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pitimbu
    City#: 357
    City Name: kaili
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kaili
    City#: 358
    City Name: ano mera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ano mera
    City#: 359
    City Name: aksarka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aksarka
    City#: 360
    City Name: vardo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=vardo
    City#: 361
    City Name: blairmore
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=blairmore
    City#: 362
    City Name: napasar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=napasar
    City#: 363
    City Name: vila do maio
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=vila do maio
    City#: 364
    City Name: kisujszallas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kisujszallas
    City#: 365
    City Name: umm kaddadah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=umm kaddadah
    City#: 366
    City Name: the valley
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=the valley
    No data for this city: {row['city']}
    City#: 367
    City Name: nizhneyansk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nizhneyansk
    City#: 368
    City Name: celestun
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=celestun
    City#: 369
    City Name: valdivia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=valdivia
    City#: 370
    City Name: caala
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=caala
    City#: 371
    City Name: valparaiso
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=valparaiso
    No data for this city: {row['city']}
    City#: 372
    City Name: koboldo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=koboldo
    City#: 373
    City Name: ciudad bolivar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ciudad bolivar
    City#: 374
    City Name: flin flon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=flin flon
    City#: 375
    City Name: sokolo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sokolo
    City#: 376
    City Name: nova olimpia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nova olimpia
    City#: 377
    City Name: nisia floresta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nisia floresta
    City#: 378
    City Name: kawalu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kawalu
    City#: 379
    City Name: groningen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=groningen
    City#: 380
    City Name: barra patuca
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=barra patuca
    City#: 381
    City Name: cizre
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cizre
    City#: 382
    City Name: dubrovnik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dubrovnik
    City#: 383
    City Name: fortuna
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=fortuna
    City#: 384
    City Name: sinnamary
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sinnamary
    City#: 385
    City Name: eureka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=eureka
    City#: 386
    City Name: san jose
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san jose
    City#: 387
    City Name: seymchan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=seymchan
    City#: 388
    City Name: tevaitoa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tevaitoa
    City#: 389
    City Name: ucluelet
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ucluelet
    City#: 390
    City Name: okhotsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=okhotsk
    City#: 391
    City Name: ler
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ler
    City#: 392
    City Name: szczytno
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=szczytno
    City#: 393
    City Name: mega
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mega
    City#: 394
    City Name: zaozerne
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zaozerne
    No data for this city: {row['city']}
    City#: 395
    City Name: bardiyah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bardiyah
    City#: 396
    City Name: namatanai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=namatanai
    City#: 397
    City Name: tilichiki
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tilichiki
    City#: 398
    City Name: babakan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=babakan
    City#: 399
    City Name: kavaratti
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kavaratti
    City#: 400
    City Name: healdsburg
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=healdsburg
    No data for this city: {row['city']}
    City#: 401
    City Name: olafsvik
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=olafsvik
    City#: 402
    City Name: sola
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sola
    City#: 403
    City Name: pio ix
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pio ix
    City#: 404
    City Name: beloha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=beloha
    City#: 405
    City Name: zyryanka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zyryanka
    City#: 406
    City Name: komsomolskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=komsomolskiy
    City#: 407
    City Name: dharchula
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dharchula
    City#: 408
    City Name: turukhansk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=turukhansk
    City#: 409
    City Name: zhuanghe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zhuanghe
    City#: 410
    City Name: santa rosa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=santa rosa
    City#: 411
    City Name: chimbote
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chimbote
    City#: 412
    City Name: abha
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=abha
    City#: 413
    City Name: bijni
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bijni
    City#: 414
    City Name: shimoda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=shimoda
    City#: 415
    City Name: margate
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=margate
    No data for this city: {row['city']}
    City#: 416
    City Name: koungou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=koungou
    No data for this city: {row['city']}
    City#: 417
    City Name: urumqi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=urumqi
    City#: 418
    City Name: ilulissat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ilulissat
    City#: 419
    City Name: belomorsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=belomorsk
    City#: 420
    City Name: fukue
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=fukue
    City#: 421
    City Name: acapulco
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=acapulco
    City#: 422
    City Name: manokwari
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=manokwari
    City#: 423
    City Name: nahan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nahan
    City#: 424
    City Name: kedrovyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kedrovyy
    City#: 425
    City Name: cayenne
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cayenne
    City#: 426
    City Name: victoria
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=victoria
    City#: 427
    City Name: necochea
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=necochea
    City#: 428
    City Name: honiara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=honiara
    City#: 429
    City Name: broome
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=broome
    City#: 430
    City Name: coracora
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=coracora
    City#: 431
    City Name: gushikawa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gushikawa
    City#: 432
    City Name: khawhai
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=khawhai
    City#: 433
    City Name: khor
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=khor
    City#: 434
    City Name: naze
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=naze
    City#: 435
    City Name: alexandria
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=alexandria
    City#: 436
    City Name: axim
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=axim
    City#: 437
    City Name: springdale
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=springdale
    City#: 438
    City Name: bathsheba
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bathsheba
    City#: 439
    City Name: stourbridge
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=stourbridge
    City#: 440
    City Name: san juan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san juan
    City#: 441
    City Name: dunedin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dunedin
    City#: 442
    City Name: ambon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ambon
    No data for this city: {row['city']}
    City#: 443
    City Name: qui nhon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=qui nhon
    City#: 444
    City Name: alofi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=alofi
    City#: 445
    City Name: mpulungu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mpulungu
    City#: 446
    City Name: sabang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sabang
    City#: 447
    City Name: carutapera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=carutapera
    City#: 448
    City Name: begun
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=begun
    City#: 449
    City Name: bayanday
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bayanday
    City#: 450
    City Name: havelock
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=havelock
    City#: 451
    City Name: san cristobal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san cristobal
    City#: 452
    City Name: katsina
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=katsina
    No data for this city: {row['city']}
    City#: 453
    City Name: chokwe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chokwe
    City#: 454
    City Name: lakes entrance
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lakes entrance
    City#: 455
    City Name: aksu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aksu
    City#: 456
    City Name: panjab
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=panjab
    City#: 457
    City Name: sur
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sur
    City#: 458
    City Name: calvinia
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=calvinia
    City#: 459
    City Name: nelson bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nelson bay
    City#: 460
    City Name: tongchuan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tongchuan
    City#: 461
    City Name: luang prabang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=luang prabang
    City#: 462
    City Name: luwingu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=luwingu
    City#: 463
    City Name: henties bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=henties bay
    City#: 464
    City Name: lugovoy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lugovoy
    City#: 465
    City Name: mathathane
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mathathane
    City#: 466
    City Name: vuktyl
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=vuktyl
    No data for this city: {row['city']}
    City#: 467
    City Name: tsihombe
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tsihombe
    City#: 468
    City Name: tezu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tezu
    City#: 469
    City Name: thiruvananthapuram
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=thiruvananthapuram
    No data for this city: {row['city']}
    City#: 470
    City Name: karauzyak
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=karauzyak
    City#: 471
    City Name: kosh-agach
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kosh-agach
    City#: 472
    City Name: gamboma
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gamboma
    City#: 473
    City Name: belaya gora
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=belaya gora
    City#: 474
    City Name: limay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=limay
    City#: 475
    City Name: bollnas
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bollnas
    City#: 476
    City Name: batsfjord
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=batsfjord
    City#: 477
    City Name: porto novo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=porto novo
    City#: 478
    City Name: mayo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mayo
    City#: 479
    City Name: ewa beach
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ewa beach
    City#: 480
    City Name: virginia beach
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=virginia beach
    City#: 481
    City Name: zaokskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zaokskiy
    City#: 482
    City Name: lata
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lata
    City#: 483
    City Name: buzmeyin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=buzmeyin
    City#: 484
    City Name: lucapa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lucapa
    City#: 485
    City Name: plettenberg bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=plettenberg bay
    City#: 486
    City Name: kajaani
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kajaani
    City#: 487
    City Name: maningrida
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=maningrida
    City#: 488
    City Name: eyl
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=eyl
    City#: 489
    City Name: goderich
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=goderich
    City#: 490
    City Name: jumla
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=jumla
    No data for this city: {row['city']}
    City#: 491
    City Name: zhitikara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zhitikara
    No data for this city: {row['city']}
    City#: 492
    City Name: chikoy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=chikoy
    City#: 493
    City Name: grand forks
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=grand forks
    City#: 494
    City Name: quatre cocos
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=quatre cocos
    City#: 495
    City Name: tombouctou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tombouctou
    City#: 496
    City Name: marsa matruh
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=marsa matruh
    City#: 497
    City Name: boyolangu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=boyolangu
    City#: 498
    City Name: zhaotong
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zhaotong
    City#: 499
    City Name: isangel
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=isangel
    City#: 500
    City Name: gorgan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gorgan
    No data for this city: {row['city']}
    City#: 501
    City Name: kazalinsk
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kazalinsk
    No data for this city: {row['city']}
    City#: 502
    City Name: tarudant
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tarudant
    City#: 503
    City Name: barstow
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=barstow
    City#: 504
    City Name: lana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lana
    City#: 505
    City Name: naberera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=naberera
    No data for this city: {row['city']}
    City#: 506
    City Name: asfi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=asfi
    City#: 507
    City Name: adrar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=adrar
    City#: 508
    City Name: novyy urgal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=novyy urgal
    No data for this city: {row['city']}
    City#: 509
    City Name: marcona
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=marcona
    City#: 510
    City Name: lerwick
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=lerwick
    City#: 511
    City Name: saint-georges
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=saint-georges
    City#: 512
    City Name: hovd
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=hovd
    City#: 513
    City Name: zheleznodorozhnyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zheleznodorozhnyy
    City#: 514
    City Name: blagoyevo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=blagoyevo
    City#: 515
    City Name: fairbanks
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=fairbanks
    City#: 516
    City Name: tevriz
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tevriz
    City#: 517
    City Name: gat
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gat
    City#: 518
    City Name: zhangye
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=zhangye
    City#: 519
    City Name: iskateley
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=iskateley
    City#: 520
    City Name: souillac
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=souillac
    City#: 521
    City Name: linkou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=linkou
    City#: 522
    City Name: safranbolu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=safranbolu
    City#: 523
    City Name: ayan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ayan
    City#: 524
    City Name: tangshan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tangshan
    City#: 525
    City Name: yumen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=yumen
    City#: 526
    City Name: bilopillya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bilopillya
    City#: 527
    City Name: sao felix do xingu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sao felix do xingu
    City#: 528
    City Name: reporoa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=reporoa
    City#: 529
    City Name: sabang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sabang
    City#: 530
    City Name: gazojak
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gazojak
    City#: 531
    City Name: kingaroy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kingaroy
    City#: 532
    City Name: dire
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dire
    City#: 533
    City Name: apac
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=apac
    City#: 534
    City Name: shieli
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=shieli
    City#: 535
    City Name: bahir dar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bahir dar
    City#: 536
    City Name: calbuco
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=calbuco
    City#: 537
    City Name: umm lajj
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=umm lajj
    City#: 538
    City Name: nome
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nome
    City#: 539
    City Name: san ramon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san ramon
    City#: 540
    City Name: la roche-sur-yon
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=la roche-sur-yon
    No data for this city: {row['city']}
    City#: 541
    City Name: rawannawi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=rawannawi
    City#: 542
    City Name: tarakan
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tarakan
    City#: 543
    City Name: nouakchott
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nouakchott
    City#: 544
    City Name: dwarka
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dwarka
    City#: 545
    City Name: bytow
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bytow
    City#: 546
    City Name: beringovskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=beringovskiy
    City#: 547
    City Name: madras
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=madras
    City#: 548
    City Name: sambava
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sambava
    City#: 549
    City Name: presidencia roque saenz pena
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=presidencia roque saenz pena
    No data for this city: {row['city']}
    City#: 550
    City Name: toftir
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=toftir
    City#: 551
    City Name: ola
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ola
    City#: 552
    City Name: qandala
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=qandala
    City#: 553
    City Name: ugento
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=ugento
    City#: 554
    City Name: enkhuizen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=enkhuizen
    City#: 555
    City Name: nema
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=nema
    City#: 556
    City Name: luganville
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=luganville
    City#: 557
    City Name: faya
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=faya
    City#: 558
    City Name: dawlatabad
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dawlatabad
    City#: 559
    City Name: soller
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=soller
    City#: 560
    City Name: san patricio
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san patricio
    City#: 561
    City Name: itarema
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=itarema
    City#: 562
    City Name: boma
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=boma
    City#: 563
    City Name: adra
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=adra
    City#: 564
    City Name: kiama
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kiama
    City#: 565
    City Name: cabedelo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cabedelo
    City#: 566
    City Name: usvyaty
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=usvyaty
    City#: 567
    City Name: kasulu
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kasulu
    City#: 568
    City Name: mujiayingzi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=mujiayingzi
    City#: 569
    City Name: yulara
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=yulara
    City#: 570
    City Name: pochutla
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=pochutla
    City#: 571
    City Name: kulunda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kulunda
    City#: 572
    City Name: praia da vitoria
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=praia da vitoria
    City#: 573
    City Name: narsaq
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=narsaq
    City#: 574
    City Name: waipawa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=waipawa
    City#: 575
    City Name: palma di montechiaro
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=palma di montechiaro
    City#: 576
    City Name: san quintin
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=san quintin
    City#: 577
    City Name: palana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=palana
    City#: 578
    City Name: fenoarivo atsinanana
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=fenoarivo atsinanana
    City#: 579
    City Name: korcula
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=korcula
    City#: 580
    City Name: oranjemund
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=oranjemund
    City#: 581
    City Name: leiyang
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=leiyang
    City#: 582
    City Name: richards bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=richards bay
    City#: 583
    City Name: cascais
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=cascais
    City#: 584
    City Name: aljezur
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=aljezur
    City#: 585
    City Name: gizo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=gizo
    City#: 586
    City Name: phalaborwa
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=phalaborwa
    City#: 587
    City Name: petropavlovsk-kamchatskiy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=petropavlovsk-kamchatskiy
    City#: 588
    City Name: egvekinot
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=egvekinot
    City#: 589
    City Name: boueni
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=boueni
    City#: 590
    City Name: tuskegee
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=tuskegee
    City#: 591
    City Name: udachnyy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=udachnyy
    City#: 592
    City Name: breytovo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=breytovo
    City#: 593
    City Name: avera
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=avera
    City#: 594
    City Name: bar harbor
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bar harbor
    City#: 595
    City Name: port lincoln
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=port lincoln
    City#: 596
    City Name: alta
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=alta
    City#: 597
    City Name: skibbereen
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=skibbereen
    City#: 598
    City Name: maralal
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=maralal
    City#: 599
    City Name: salalah
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=salalah
    City#: 600
    City Name: sobolevo
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=sobolevo
    City#: 601
    City Name: kokoda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=kokoda
    City#: 602
    City Name: dingzhou
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dingzhou
    City#: 603
    City Name: igunga
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=igunga
    City#: 604
    City Name: batemans bay
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=batemans bay
    City#: 605
    City Name: port hardy
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=port hardy
    City#: 606
    City Name: dunda
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=dunda
    City#: 607
    City Name: marawi
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=marawi
    No data for this city: {row['city']}
    City#: 608
    City Name: warqla
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=warqla
    City#: 609
    City Name: bakchar
    URL: http://api.openweathermap.org/data/2.5/weather?appid=13eb285e756e4390109c44f34ee69883&units=Imperial&q=bakchar



```python
#test data
#location_data['temperature']
#location_data['humidity'] 
#location_data['cloudiness']
#location_data['wind_speed'] 
#location_data.head()
#NOTE BLANK CELLS - DELETE: https://stackoverflow.com/questions/29314033/python-pandas-dataframe-remove-empty-cells
location_update = location_data.replace('', np.nan, inplace=True)
location_update = location_data.dropna()   
location_update.head()
print(len(location_update))
location_update
```

    544





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
      <th>latitude</th>
      <th>longitude</th>
      <th>city</th>
      <th>country</th>
      <th>temperature</th>
      <th>humidity</th>
      <th>cloudiness</th>
      <th>wind_speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.179579</td>
      <td>52.596091</td>
      <td>abu dhabi</td>
      <td>ae</td>
      <td>72.0</td>
      <td>53.0</td>
      <td>0.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17.783145</td>
      <td>-132.161067</td>
      <td>lompoc</td>
      <td>us</td>
      <td>58.0</td>
      <td>47.0</td>
      <td>1.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-71.939904</td>
      <td>-20.524464</td>
      <td>mar del plata</td>
      <td>ar</td>
      <td>67.0</td>
      <td>41.0</td>
      <td>68.0</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-60.765482</td>
      <td>-116.577137</td>
      <td>rikitea</td>
      <td>pf</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>56.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>28.617038</td>
      <td>-91.546826</td>
      <td>morgan city</td>
      <td>us</td>
      <td>77.0</td>
      <td>78.0</td>
      <td>90.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>-82.080262</td>
      <td>-178.843105</td>
      <td>vaini</td>
      <td>to</td>
      <td>66.0</td>
      <td>91.0</td>
      <td>8.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>11.626859</td>
      <td>92.334210</td>
      <td>port blair</td>
      <td>in</td>
      <td>82.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>-77.615603</td>
      <td>-86.826217</td>
      <td>punta arenas</td>
      <td>cl</td>
      <td>46.0</td>
      <td>100.0</td>
      <td>75.0</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>-68.923262</td>
      <td>-66.663244</td>
      <td>ushuaia</td>
      <td>ar</td>
      <td>41.0</td>
      <td>80.0</td>
      <td>75.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>-78.236913</td>
      <td>-143.232160</td>
      <td>mataura</td>
      <td>pf</td>
      <td>66.0</td>
      <td>64.0</td>
      <td>100.0</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>-45.412624</td>
      <td>21.678038</td>
      <td>bredasdorp</td>
      <td>za</td>
      <td>62.0</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>-55.762548</td>
      <td>164.843432</td>
      <td>bluff</td>
      <td>nz</td>
      <td>72.0</td>
      <td>90.0</td>
      <td>24.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>31.906230</td>
      <td>132.150381</td>
      <td>shintomi</td>
      <td>jp</td>
      <td>59.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>-12.817160</td>
      <td>-44.058274</td>
      <td>santa maria da vitoria</td>
      <td>br</td>
      <td>79.0</td>
      <td>76.0</td>
      <td>8.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>88.595528</td>
      <td>-70.195662</td>
      <td>qaanaaq</td>
      <td>gl</td>
      <td>-17.0</td>
      <td>98.0</td>
      <td>0.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>-13.574405</td>
      <td>-6.684830</td>
      <td>jamestown</td>
      <td>sh</td>
      <td>44.0</td>
      <td>82.0</td>
      <td>0.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>-67.246705</td>
      <td>44.832425</td>
      <td>port alfred</td>
      <td>za</td>
      <td>70.0</td>
      <td>86.0</td>
      <td>0.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>-75.654722</td>
      <td>118.510794</td>
      <td>albany</td>
      <td>au</td>
      <td>28.0</td>
      <td>30.0</td>
      <td>20.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>-67.614511</td>
      <td>-9.625817</td>
      <td>cape town</td>
      <td>za</td>
      <td>66.0</td>
      <td>88.0</td>
      <td>20.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>38.938630</td>
      <td>-38.070964</td>
      <td>ribeira grande</td>
      <td>pt</td>
      <td>62.0</td>
      <td>97.0</td>
      <td>20.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>56.018300</td>
      <td>10.135039</td>
      <td>solbjerg</td>
      <td>dk</td>
      <td>23.0</td>
      <td>85.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>-39.061961</td>
      <td>-105.347811</td>
      <td>ancud</td>
      <td>cl</td>
      <td>56.0</td>
      <td>85.0</td>
      <td>80.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>33.918585</td>
      <td>133.684373</td>
      <td>ikeda</td>
      <td>jp</td>
      <td>55.0</td>
      <td>82.0</td>
      <td>90.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>52.491907</td>
      <td>-76.447186</td>
      <td>matagami</td>
      <td>ca</td>
      <td>71.0</td>
      <td>33.0</td>
      <td>24.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>-65.936918</td>
      <td>8.087790</td>
      <td>hermanus</td>
      <td>za</td>
      <td>59.0</td>
      <td>93.0</td>
      <td>80.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>39.772347</td>
      <td>109.067509</td>
      <td>dongsheng</td>
      <td>cn</td>
      <td>44.0</td>
      <td>94.0</td>
      <td>80.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>37</th>
      <td>26.495878</td>
      <td>21.662140</td>
      <td>jalu</td>
      <td>ly</td>
      <td>63.0</td>
      <td>37.0</td>
      <td>0.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>35.790542</td>
      <td>178.378947</td>
      <td>nikolskoye</td>
      <td>ru</td>
      <td>30.0</td>
      <td>80.0</td>
      <td>20.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>39</th>
      <td>74.847351</td>
      <td>169.650131</td>
      <td>pevek</td>
      <td>ru</td>
      <td>-19.0</td>
      <td>100.0</td>
      <td>24.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>37.112820</td>
      <td>77.473639</td>
      <td>shache</td>
      <td>cn</td>
      <td>36.0</td>
      <td>77.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1401</th>
      <td>42.241918</td>
      <td>17.423458</td>
      <td>korcula</td>
      <td>hr</td>
      <td>53.0</td>
      <td>71.0</td>
      <td>75.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>1403</th>
      <td>-30.157757</td>
      <td>12.359310</td>
      <td>oranjemund</td>
      <td>na</td>
      <td>62.0</td>
      <td>94.0</td>
      <td>92.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>1405</th>
      <td>26.440739</td>
      <td>113.313142</td>
      <td>leiyang</td>
      <td>cn</td>
      <td>47.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1408</th>
      <td>-31.816478</td>
      <td>37.350725</td>
      <td>richards bay</td>
      <td>za</td>
      <td>69.0</td>
      <td>82.0</td>
      <td>8.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1414</th>
      <td>38.319573</td>
      <td>-10.035126</td>
      <td>cascais</td>
      <td>pt</td>
      <td>52.0</td>
      <td>76.0</td>
      <td>20.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1425</th>
      <td>37.343324</td>
      <td>-10.495773</td>
      <td>aljezur</td>
      <td>pt</td>
      <td>55.0</td>
      <td>100.0</td>
      <td>36.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>1429</th>
      <td>-7.116976</td>
      <td>157.579892</td>
      <td>gizo</td>
      <td>sb</td>
      <td>68.0</td>
      <td>42.0</td>
      <td>0.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1430</th>
      <td>-23.820978</td>
      <td>31.725297</td>
      <td>phalaborwa</td>
      <td>za</td>
      <td>66.0</td>
      <td>83.0</td>
      <td>0.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1432</th>
      <td>49.946172</td>
      <td>159.694625</td>
      <td>petropavlovsk-kamchatskiy</td>
      <td>ru</td>
      <td>8.0</td>
      <td>65.0</td>
      <td>12.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1433</th>
      <td>58.542238</td>
      <td>-178.966948</td>
      <td>egvekinot</td>
      <td>ru</td>
      <td>-12.0</td>
      <td>75.0</td>
      <td>64.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1434</th>
      <td>-14.135595</td>
      <td>43.799788</td>
      <td>boueni</td>
      <td>yt</td>
      <td>82.0</td>
      <td>83.0</td>
      <td>92.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1436</th>
      <td>32.327097</td>
      <td>-85.427829</td>
      <td>tuskegee</td>
      <td>us</td>
      <td>75.0</td>
      <td>64.0</td>
      <td>1.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1438</th>
      <td>66.055367</td>
      <td>112.535667</td>
      <td>udachnyy</td>
      <td>ru</td>
      <td>-5.0</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1439</th>
      <td>58.694228</td>
      <td>38.118565</td>
      <td>breytovo</td>
      <td>ru</td>
      <td>15.0</td>
      <td>80.0</td>
      <td>44.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>1443</th>
      <td>-32.608820</td>
      <td>-155.820621</td>
      <td>avera</td>
      <td>pf</td>
      <td>79.0</td>
      <td>19.0</td>
      <td>1.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1448</th>
      <td>43.457556</td>
      <td>-67.434052</td>
      <td>bar harbor</td>
      <td>us</td>
      <td>25.0</td>
      <td>35.0</td>
      <td>1.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>1449</th>
      <td>-39.805272</td>
      <td>134.705639</td>
      <td>port lincoln</td>
      <td>au</td>
      <td>65.0</td>
      <td>91.0</td>
      <td>88.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>69.695678</td>
      <td>23.085081</td>
      <td>alta</td>
      <td>no</td>
      <td>25.0</td>
      <td>73.0</td>
      <td>75.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>51.027351</td>
      <td>-9.109588</td>
      <td>skibbereen</td>
      <td>ie</td>
      <td>32.0</td>
      <td>100.0</td>
      <td>75.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>1.408739</td>
      <td>36.077910</td>
      <td>maralal</td>
      <td>ke</td>
      <td>58.0</td>
      <td>96.0</td>
      <td>24.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1473</th>
      <td>13.093113</td>
      <td>58.842932</td>
      <td>salalah</td>
      <td>om</td>
      <td>77.0</td>
      <td>73.0</td>
      <td>20.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1474</th>
      <td>54.674609</td>
      <td>154.477719</td>
      <td>sobolevo</td>
      <td>ru</td>
      <td>8.0</td>
      <td>71.0</td>
      <td>0.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1475</th>
      <td>-8.884503</td>
      <td>147.947003</td>
      <td>kokoda</td>
      <td>pg</td>
      <td>62.0</td>
      <td>99.0</td>
      <td>92.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1482</th>
      <td>39.432845</td>
      <td>114.414717</td>
      <td>dingzhou</td>
      <td>cn</td>
      <td>37.0</td>
      <td>100.0</td>
      <td>48.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1483</th>
      <td>-4.594141</td>
      <td>33.720679</td>
      <td>igunga</td>
      <td>tz</td>
      <td>64.0</td>
      <td>90.0</td>
      <td>56.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1485</th>
      <td>-39.740214</td>
      <td>153.571656</td>
      <td>batemans bay</td>
      <td>au</td>
      <td>61.0</td>
      <td>99.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1486</th>
      <td>43.984494</td>
      <td>-138.929020</td>
      <td>port hardy</td>
      <td>ca</td>
      <td>44.0</td>
      <td>81.0</td>
      <td>20.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1490</th>
      <td>-7.336343</td>
      <td>33.495293</td>
      <td>dunda</td>
      <td>tz</td>
      <td>43.0</td>
      <td>56.0</td>
      <td>8.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1491</th>
      <td>16.867243</td>
      <td>28.912400</td>
      <td>marawi</td>
      <td>sd</td>
      <td>63.0</td>
      <td>92.0</td>
      <td>20.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1494</th>
      <td>56.618849</td>
      <td>81.042911</td>
      <td>bakchar</td>
      <td>ru</td>
      <td>23.0</td>
      <td>89.0</td>
      <td>8.0</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
<p>544 rows × 8 columns</p>
</div>




```python
plt.scatter(location_update['latitude'], location_update['temperature'])
plt.title("Temperature(f) V. Latitude")
plt.xlabel("Latitude")
plt.ylabel("Temperature")
plt.savefig("Temp_v_Latitude.png")
```


![png](output_7_0.png)



```python
plt.scatter(location_update['latitude'], location_update['humidity'])
plt.title("Humidity V. Latitude")
plt.xlabel("Latitude")
plt.ylabel("Humidity")
plt.savefig("Humidity_v_Latitude.png")
```


![png](output_8_0.png)



```python
plt.scatter(location_update['latitude'], location_update['cloudiness'])
plt.title("Cloudiness V. Latitude")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness")
plt.savefig("Cloudiness_v_Latitude.png")
```


![png](output_9_0.png)



```python
plt.scatter(location_update['latitude'], location_update['wind_speed'])
plt.title("Wind Speed V. Latitude")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed")
plt.savefig("Wind_v_Latitude.png")
```


![png](output_10_0.png)



```python
location_update.to_csv('Cities.csv')
```


```python
location_update
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
      <th>latitude</th>
      <th>longitude</th>
      <th>city</th>
      <th>country</th>
      <th>temperature</th>
      <th>humidity</th>
      <th>cloudiness</th>
      <th>wind_speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22.179579</td>
      <td>52.596091</td>
      <td>abu dhabi</td>
      <td>ae</td>
      <td>72.0</td>
      <td>53.0</td>
      <td>0.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17.783145</td>
      <td>-132.161067</td>
      <td>lompoc</td>
      <td>us</td>
      <td>58.0</td>
      <td>47.0</td>
      <td>1.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-71.939904</td>
      <td>-20.524464</td>
      <td>mar del plata</td>
      <td>ar</td>
      <td>67.0</td>
      <td>41.0</td>
      <td>68.0</td>
      <td>29.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-60.765482</td>
      <td>-116.577137</td>
      <td>rikitea</td>
      <td>pf</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>56.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>28.617038</td>
      <td>-91.546826</td>
      <td>morgan city</td>
      <td>us</td>
      <td>77.0</td>
      <td>78.0</td>
      <td>90.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>-82.080262</td>
      <td>-178.843105</td>
      <td>vaini</td>
      <td>to</td>
      <td>66.0</td>
      <td>91.0</td>
      <td>8.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>11.626859</td>
      <td>92.334210</td>
      <td>port blair</td>
      <td>in</td>
      <td>82.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>-77.615603</td>
      <td>-86.826217</td>
      <td>punta arenas</td>
      <td>cl</td>
      <td>46.0</td>
      <td>100.0</td>
      <td>75.0</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>-68.923262</td>
      <td>-66.663244</td>
      <td>ushuaia</td>
      <td>ar</td>
      <td>41.0</td>
      <td>80.0</td>
      <td>75.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>-78.236913</td>
      <td>-143.232160</td>
      <td>mataura</td>
      <td>pf</td>
      <td>66.0</td>
      <td>64.0</td>
      <td>100.0</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>-45.412624</td>
      <td>21.678038</td>
      <td>bredasdorp</td>
      <td>za</td>
      <td>62.0</td>
      <td>88.0</td>
      <td>88.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>-55.762548</td>
      <td>164.843432</td>
      <td>bluff</td>
      <td>nz</td>
      <td>72.0</td>
      <td>90.0</td>
      <td>24.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>31.906230</td>
      <td>132.150381</td>
      <td>shintomi</td>
      <td>jp</td>
      <td>59.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>-12.817160</td>
      <td>-44.058274</td>
      <td>santa maria da vitoria</td>
      <td>br</td>
      <td>79.0</td>
      <td>76.0</td>
      <td>8.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>88.595528</td>
      <td>-70.195662</td>
      <td>qaanaaq</td>
      <td>gl</td>
      <td>-17.0</td>
      <td>98.0</td>
      <td>0.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>-13.574405</td>
      <td>-6.684830</td>
      <td>jamestown</td>
      <td>sh</td>
      <td>44.0</td>
      <td>82.0</td>
      <td>0.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>-67.246705</td>
      <td>44.832425</td>
      <td>port alfred</td>
      <td>za</td>
      <td>70.0</td>
      <td>86.0</td>
      <td>0.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>-75.654722</td>
      <td>118.510794</td>
      <td>albany</td>
      <td>au</td>
      <td>28.0</td>
      <td>30.0</td>
      <td>20.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>-67.614511</td>
      <td>-9.625817</td>
      <td>cape town</td>
      <td>za</td>
      <td>66.0</td>
      <td>88.0</td>
      <td>20.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>38.938630</td>
      <td>-38.070964</td>
      <td>ribeira grande</td>
      <td>pt</td>
      <td>62.0</td>
      <td>97.0</td>
      <td>20.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>56.018300</td>
      <td>10.135039</td>
      <td>solbjerg</td>
      <td>dk</td>
      <td>23.0</td>
      <td>85.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>-39.061961</td>
      <td>-105.347811</td>
      <td>ancud</td>
      <td>cl</td>
      <td>56.0</td>
      <td>85.0</td>
      <td>80.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>33.918585</td>
      <td>133.684373</td>
      <td>ikeda</td>
      <td>jp</td>
      <td>55.0</td>
      <td>82.0</td>
      <td>90.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>52.491907</td>
      <td>-76.447186</td>
      <td>matagami</td>
      <td>ca</td>
      <td>71.0</td>
      <td>33.0</td>
      <td>24.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>-65.936918</td>
      <td>8.087790</td>
      <td>hermanus</td>
      <td>za</td>
      <td>59.0</td>
      <td>93.0</td>
      <td>80.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>39.772347</td>
      <td>109.067509</td>
      <td>dongsheng</td>
      <td>cn</td>
      <td>44.0</td>
      <td>94.0</td>
      <td>80.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>37</th>
      <td>26.495878</td>
      <td>21.662140</td>
      <td>jalu</td>
      <td>ly</td>
      <td>63.0</td>
      <td>37.0</td>
      <td>0.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>35.790542</td>
      <td>178.378947</td>
      <td>nikolskoye</td>
      <td>ru</td>
      <td>30.0</td>
      <td>80.0</td>
      <td>20.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>39</th>
      <td>74.847351</td>
      <td>169.650131</td>
      <td>pevek</td>
      <td>ru</td>
      <td>-19.0</td>
      <td>100.0</td>
      <td>24.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>37.112820</td>
      <td>77.473639</td>
      <td>shache</td>
      <td>cn</td>
      <td>36.0</td>
      <td>77.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1401</th>
      <td>42.241918</td>
      <td>17.423458</td>
      <td>korcula</td>
      <td>hr</td>
      <td>53.0</td>
      <td>71.0</td>
      <td>75.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>1403</th>
      <td>-30.157757</td>
      <td>12.359310</td>
      <td>oranjemund</td>
      <td>na</td>
      <td>62.0</td>
      <td>94.0</td>
      <td>92.0</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>1405</th>
      <td>26.440739</td>
      <td>113.313142</td>
      <td>leiyang</td>
      <td>cn</td>
      <td>47.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1408</th>
      <td>-31.816478</td>
      <td>37.350725</td>
      <td>richards bay</td>
      <td>za</td>
      <td>69.0</td>
      <td>82.0</td>
      <td>8.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1414</th>
      <td>38.319573</td>
      <td>-10.035126</td>
      <td>cascais</td>
      <td>pt</td>
      <td>52.0</td>
      <td>76.0</td>
      <td>20.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1425</th>
      <td>37.343324</td>
      <td>-10.495773</td>
      <td>aljezur</td>
      <td>pt</td>
      <td>55.0</td>
      <td>100.0</td>
      <td>36.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>1429</th>
      <td>-7.116976</td>
      <td>157.579892</td>
      <td>gizo</td>
      <td>sb</td>
      <td>68.0</td>
      <td>42.0</td>
      <td>0.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1430</th>
      <td>-23.820978</td>
      <td>31.725297</td>
      <td>phalaborwa</td>
      <td>za</td>
      <td>66.0</td>
      <td>83.0</td>
      <td>0.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1432</th>
      <td>49.946172</td>
      <td>159.694625</td>
      <td>petropavlovsk-kamchatskiy</td>
      <td>ru</td>
      <td>8.0</td>
      <td>65.0</td>
      <td>12.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1433</th>
      <td>58.542238</td>
      <td>-178.966948</td>
      <td>egvekinot</td>
      <td>ru</td>
      <td>-12.0</td>
      <td>75.0</td>
      <td>64.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1434</th>
      <td>-14.135595</td>
      <td>43.799788</td>
      <td>boueni</td>
      <td>yt</td>
      <td>82.0</td>
      <td>83.0</td>
      <td>92.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1436</th>
      <td>32.327097</td>
      <td>-85.427829</td>
      <td>tuskegee</td>
      <td>us</td>
      <td>75.0</td>
      <td>64.0</td>
      <td>1.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1438</th>
      <td>66.055367</td>
      <td>112.535667</td>
      <td>udachnyy</td>
      <td>ru</td>
      <td>-5.0</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>1439</th>
      <td>58.694228</td>
      <td>38.118565</td>
      <td>breytovo</td>
      <td>ru</td>
      <td>15.0</td>
      <td>80.0</td>
      <td>44.0</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>1443</th>
      <td>-32.608820</td>
      <td>-155.820621</td>
      <td>avera</td>
      <td>pf</td>
      <td>79.0</td>
      <td>19.0</td>
      <td>1.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>1448</th>
      <td>43.457556</td>
      <td>-67.434052</td>
      <td>bar harbor</td>
      <td>us</td>
      <td>25.0</td>
      <td>35.0</td>
      <td>1.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>1449</th>
      <td>-39.805272</td>
      <td>134.705639</td>
      <td>port lincoln</td>
      <td>au</td>
      <td>65.0</td>
      <td>91.0</td>
      <td>88.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>69.695678</td>
      <td>23.085081</td>
      <td>alta</td>
      <td>no</td>
      <td>25.0</td>
      <td>73.0</td>
      <td>75.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>51.027351</td>
      <td>-9.109588</td>
      <td>skibbereen</td>
      <td>ie</td>
      <td>32.0</td>
      <td>100.0</td>
      <td>75.0</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>1.408739</td>
      <td>36.077910</td>
      <td>maralal</td>
      <td>ke</td>
      <td>58.0</td>
      <td>96.0</td>
      <td>24.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1473</th>
      <td>13.093113</td>
      <td>58.842932</td>
      <td>salalah</td>
      <td>om</td>
      <td>77.0</td>
      <td>73.0</td>
      <td>20.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1474</th>
      <td>54.674609</td>
      <td>154.477719</td>
      <td>sobolevo</td>
      <td>ru</td>
      <td>8.0</td>
      <td>71.0</td>
      <td>0.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>1475</th>
      <td>-8.884503</td>
      <td>147.947003</td>
      <td>kokoda</td>
      <td>pg</td>
      <td>62.0</td>
      <td>99.0</td>
      <td>92.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1482</th>
      <td>39.432845</td>
      <td>114.414717</td>
      <td>dingzhou</td>
      <td>cn</td>
      <td>37.0</td>
      <td>100.0</td>
      <td>48.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1483</th>
      <td>-4.594141</td>
      <td>33.720679</td>
      <td>igunga</td>
      <td>tz</td>
      <td>64.0</td>
      <td>90.0</td>
      <td>56.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1485</th>
      <td>-39.740214</td>
      <td>153.571656</td>
      <td>batemans bay</td>
      <td>au</td>
      <td>61.0</td>
      <td>99.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1486</th>
      <td>43.984494</td>
      <td>-138.929020</td>
      <td>port hardy</td>
      <td>ca</td>
      <td>44.0</td>
      <td>81.0</td>
      <td>20.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1490</th>
      <td>-7.336343</td>
      <td>33.495293</td>
      <td>dunda</td>
      <td>tz</td>
      <td>43.0</td>
      <td>56.0</td>
      <td>8.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1491</th>
      <td>16.867243</td>
      <td>28.912400</td>
      <td>marawi</td>
      <td>sd</td>
      <td>63.0</td>
      <td>92.0</td>
      <td>20.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1494</th>
      <td>56.618849</td>
      <td>81.042911</td>
      <td>bakchar</td>
      <td>ru</td>
      <td>23.0</td>
      <td>89.0</td>
      <td>8.0</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
<p>544 rows × 8 columns</p>
</div>




```python
#plt.scatter(location_update['longitude'], location_update['cloudiness'])
#plt.title("Cloudiness V. Longitude")
#plt.xlabel("Longitude")
#plt.ylabel("Cloudiness")
```


```python
plt.scatter(location_update['humidity'], location_update['cloudiness'])
plt.title("humdity V. cloudiness")
plt.xlabel("cloudiness")
plt.ylabel("humidity")
plt.savefig("Wind_v_Latitude.png")
plt.savefig("Humidity_V_Cloudiness.png")
```


![png](output_14_0.png)

