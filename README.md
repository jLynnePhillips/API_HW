
Observation 1: Of the randomly selected cities that the OpenWeatherMap API was able to get weather data for, more have latitudes between 50 degrees and 90 degrees than have latitudes between -50 degrees and -90 degrees.
Observation 2: The temperature appears to drop as the latitude approaches 90 degrees, but not as it approaches -90 degrees.
Observation 3: Distance from the equator does not have a noticeable effect on humidity, cloudiness, or windspeed.


```python
# Dependencies
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import requests
import random
import time
from citipy import citipy
import seaborn as sns
sns.set()

# OpenWeatherMap API Key from config
from config import api_key
```


```python
# Randomly select 700 unique cities

# Create lists to store randomly selected latitudes, longitudes, and cities
lat = []
lng = []
cities = []

# Continue untill 700 unique cities have been selected
while len(cities) < 700:
    # Randomly select latitude and longitude
    test_lat = random.randint(-9000,9000)/100.
    test_lng = random.randint(-18000,18000)/100.
    # Use citipy to get city names
    city_data = citipy.nearest_city(test_lat, test_lng)
    city = city_data.city_name
    # Only append cities not already in the list
    if city not in cities:
        cities.append(city)
        lat.append(test_lat)
        lng.append(test_lng)
```


```python
# Create a pandas dataframe with all desired columns
weather_df = pd.DataFrame({"latitude":lat,"longitude":lng,"city":cities})
weather_df["temperature"] = np.nan
weather_df["humidity"] = np.nan
weather_df["cloudiness"] = np.nan
weather_df["wind speed"] = np.nan
```


```python
# Use the openweathermap api

# Counter
row_count = 0

# Loop through the cities and get the weather information
for index, row in weather_df.iterrows():
    
    # Create endpoint URL
    query_url = "http://api.openweathermap.org/data/2.5/weather?appid=%s&q=%s&units=imperial" % (api_key, row["city"])
    # Remove the api key from the URL that prints to log
    print_url = "http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=%s&units=imperial" % row["city"]
    
    # Print log to ensure the loop is working correctly
    print(f"Now retrieving city # {row_count}, {row['city']}")
    print(print_url)
    row_count += 1
    
    # Grab JSON at the openweathermap api
    weather_response = requests.get(query_url).json()
    # Use try/except to skip any cities with weather errors
    try:
        test_temp = weather_response["main"]["temp_max"]
        test_humidity = weather_response["main"]["humidity"]
        test_wind = weather_response["wind"]["speed"]
        test_clouds = weather_response["clouds"]["all"]
        weather_df.set_value(index,"temperature", test_temp)
        weather_df.set_value(index,"humidity", test_humidity)
        weather_df.set_value(index,"cloudiness", test_clouds)
        weather_df.set_value(index,"wind speed", test_wind)
    except:
        print("Error with city weather data. Skipping")
        continue
    # Half second time delay between api hits
    time.sleep(0.5)
    
```

    Now retrieving city # 0, bow island
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bow island&units=imperial
    Now retrieving city # 1, bethel
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bethel&units=imperial
    Now retrieving city # 2, bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bengkulu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 3, east london
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=east london&units=imperial
    Now retrieving city # 4, fort nelson
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=fort nelson&units=imperial
    Now retrieving city # 5, chuy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chuy&units=imperial
    Now retrieving city # 6, anicuns
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=anicuns&units=imperial
    Now retrieving city # 7, jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jamestown&units=imperial
    Now retrieving city # 8, puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=puerto ayora&units=imperial
    Now retrieving city # 9, busselton
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=busselton&units=imperial
    Now retrieving city # 10, college
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=college&units=imperial
    Now retrieving city # 11, longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=longyearbyen&units=imperial
    Now retrieving city # 12, yulara
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yulara&units=imperial
    Now retrieving city # 13, kungurtug
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kungurtug&units=imperial
    Now retrieving city # 14, cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cidreira&units=imperial
    Now retrieving city # 15, punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=punta arenas&units=imperial
    Now retrieving city # 16, severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=severo-kurilsk&units=imperial
    Now retrieving city # 17, bartica
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bartica&units=imperial
    Now retrieving city # 18, kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kapaa&units=imperial
    Now retrieving city # 19, cape town
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cape town&units=imperial
    Now retrieving city # 20, nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nikolskoye&units=imperial
    Now retrieving city # 21, warmbad
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=warmbad&units=imperial
    Now retrieving city # 22, ancud
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ancud&units=imperial
    Now retrieving city # 23, coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=coquimbo&units=imperial
    Now retrieving city # 24, provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=provideniya&units=imperial
    Now retrieving city # 25, emerald
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=emerald&units=imperial
    Now retrieving city # 26, karpathos
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=karpathos&units=imperial
    Now retrieving city # 27, tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tuktoyaktuk&units=imperial
    Now retrieving city # 28, port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port elizabeth&units=imperial
    Now retrieving city # 29, whyalla
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=whyalla&units=imperial
    Now retrieving city # 30, khani
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=khani&units=imperial
    Now retrieving city # 31, marsassoum
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=marsassoum&units=imperial
    Now retrieving city # 32, hofn
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hofn&units=imperial
    Now retrieving city # 33, albany
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=albany&units=imperial
    Now retrieving city # 34, lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lorengau&units=imperial
    Now retrieving city # 35, yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yellowknife&units=imperial
    Now retrieving city # 36, dikson
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dikson&units=imperial
    Now retrieving city # 37, sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sentyabrskiy&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 38, vaini
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vaini&units=imperial
    Now retrieving city # 39, nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nanortalik&units=imperial
    Now retrieving city # 40, samusu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=samusu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 41, ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ilulissat&units=imperial
    Now retrieving city # 42, rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rikitea&units=imperial
    Now retrieving city # 43, yumen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yumen&units=imperial
    Now retrieving city # 44, os
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=os&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 45, lomovka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lomovka&units=imperial
    Now retrieving city # 46, foshan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=foshan&units=imperial
    Now retrieving city # 47, aykhal
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aykhal&units=imperial
    Now retrieving city # 48, palmer
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=palmer&units=imperial
    Now retrieving city # 49, axim
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=axim&units=imperial
    Now retrieving city # 50, guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=guerrero negro&units=imperial
    Now retrieving city # 51, cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cherskiy&units=imperial
    Now retrieving city # 52, samarai
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=samarai&units=imperial
    Now retrieving city # 53, hilo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hilo&units=imperial
    Now retrieving city # 54, barmer
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=barmer&units=imperial
    Now retrieving city # 55, beni suef
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=beni suef&units=imperial
    Now retrieving city # 56, port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port alfred&units=imperial
    Now retrieving city # 57, luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=luderitz&units=imperial
    Now retrieving city # 58, butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=butaritari&units=imperial
    Now retrieving city # 59, mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mar del plata&units=imperial
    Now retrieving city # 60, changtu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=changtu&units=imperial
    Now retrieving city # 61, davila
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=davila&units=imperial
    Now retrieving city # 62, shitanjing
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shitanjing&units=imperial
    Now retrieving city # 63, clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=clyde river&units=imperial
    Now retrieving city # 64, new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=new norfolk&units=imperial
    Now retrieving city # 65, salalah
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=salalah&units=imperial
    Now retrieving city # 66, barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=barentsburg&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 67, kindu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kindu&units=imperial
    Now retrieving city # 68, daru
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=daru&units=imperial
    Now retrieving city # 69, laguna
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=laguna&units=imperial
    Now retrieving city # 70, santa clara
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=santa clara&units=imperial
    Now retrieving city # 71, port hardy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port hardy&units=imperial
    Now retrieving city # 72, tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tuatapere&units=imperial
    Now retrieving city # 73, santiago del estero
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=santiago del estero&units=imperial
    Now retrieving city # 74, souillac
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=souillac&units=imperial
    Now retrieving city # 75, bilibino
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bilibino&units=imperial
    Now retrieving city # 76, grand gaube
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=grand gaube&units=imperial
    Now retrieving city # 77, ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ponta do sol&units=imperial
    Now retrieving city # 78, conakry
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=conakry&units=imperial
    Now retrieving city # 79, airai
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=airai&units=imperial
    Now retrieving city # 80, qaqortoq
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=qaqortoq&units=imperial
    Now retrieving city # 81, narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=narsaq&units=imperial
    Now retrieving city # 82, ponta delgada
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ponta delgada&units=imperial
    Now retrieving city # 83, san marco in lamis
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san marco in lamis&units=imperial
    Now retrieving city # 84, chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chokurdakh&units=imperial
    Now retrieving city # 85, kamenka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kamenka&units=imperial
    Now retrieving city # 86, mount isa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mount isa&units=imperial
    Now retrieving city # 87, imbituba
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=imbituba&units=imperial
    Now retrieving city # 88, barrow
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=barrow&units=imperial
    Now retrieving city # 89, cruzeiro do sul
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cruzeiro do sul&units=imperial
    Now retrieving city # 90, muros
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=muros&units=imperial
    Now retrieving city # 91, iberia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=iberia&units=imperial
    Now retrieving city # 92, panzhihua
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=panzhihua&units=imperial
    Now retrieving city # 93, saryshagan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saryshagan&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 94, upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=upernavik&units=imperial
    Now retrieving city # 95, baykit
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=baykit&units=imperial
    Now retrieving city # 96, nuuk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nuuk&units=imperial
    Now retrieving city # 97, arawa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=arawa&units=imperial
    Now retrieving city # 98, loding
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=loding&units=imperial
    Now retrieving city # 99, torbay
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=torbay&units=imperial
    Now retrieving city # 100, katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=katsuura&units=imperial
    Now retrieving city # 101, atuona
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=atuona&units=imperial
    Now retrieving city # 102, hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hermanus&units=imperial
    Now retrieving city # 103, ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ushuaia&units=imperial
    Now retrieving city # 104, puri
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=puri&units=imperial
    Now retrieving city # 105, udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=udachnyy&units=imperial
    Now retrieving city # 106, mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mys shmidta&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 107, hidalgotitlan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hidalgotitlan&units=imperial
    Now retrieving city # 108, lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lompoc&units=imperial
    Now retrieving city # 109, mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mahebourg&units=imperial
    Now retrieving city # 110, san pedro de macoris
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san pedro de macoris&units=imperial
    Now retrieving city # 111, tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tasiilaq&units=imperial
    Now retrieving city # 112, trairi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=trairi&units=imperial
    Now retrieving city # 113, te anau
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=te anau&units=imperial
    Now retrieving city # 114, mataura
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mataura&units=imperial
    Now retrieving city # 115, carutapera
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=carutapera&units=imperial
    Now retrieving city # 116, lake city
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lake city&units=imperial
    Now retrieving city # 117, hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hamilton&units=imperial
    Now retrieving city # 118, ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ostrovnoy&units=imperial
    Now retrieving city # 119, faanui
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=faanui&units=imperial
    Now retrieving city # 120, arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=arraial do cabo&units=imperial
    Now retrieving city # 121, burica
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=burica&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 122, port keats
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port keats&units=imperial
    Now retrieving city # 123, mayo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mayo&units=imperial
    Now retrieving city # 124, lebu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lebu&units=imperial
    Now retrieving city # 125, tabiauea
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tabiauea&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 126, avarua
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=avarua&units=imperial
    Now retrieving city # 127, hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hasaki&units=imperial
    Now retrieving city # 128, parfenyevo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=parfenyevo&units=imperial
    Now retrieving city # 129, madingou
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=madingou&units=imperial
    Now retrieving city # 130, tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tilichiki&units=imperial
    Now retrieving city # 131, leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=leningradskiy&units=imperial
    Now retrieving city # 132, belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=belushya guba&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 133, saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saldanha&units=imperial
    Now retrieving city # 134, alice springs
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=alice springs&units=imperial
    Now retrieving city # 135, roma
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=roma&units=imperial
    Now retrieving city # 136, kestel
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kestel&units=imperial
    Now retrieving city # 137, espinosa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=espinosa&units=imperial
    Now retrieving city # 138, atar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=atar&units=imperial
    Now retrieving city # 139, bluff
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bluff&units=imperial
    Now retrieving city # 140, pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pangnirtung&units=imperial
    Now retrieving city # 141, codrington
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=codrington&units=imperial
    Now retrieving city # 142, killarney
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=killarney&units=imperial
    Now retrieving city # 143, chernyshevskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chernyshevskiy&units=imperial
    Now retrieving city # 144, bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bambous virieux&units=imperial
    Now retrieving city # 145, teguise
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=teguise&units=imperial
    Now retrieving city # 146, tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tsihombe&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 147, romanovo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=romanovo&units=imperial
    Now retrieving city # 148, faya
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=faya&units=imperial
    Now retrieving city # 149, high rock
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=high rock&units=imperial
    Now retrieving city # 150, castro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=castro&units=imperial
    Now retrieving city # 151, qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=qaanaaq&units=imperial
    Now retrieving city # 152, omboue
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=omboue&units=imperial
    Now retrieving city # 153, ures
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ures&units=imperial
    Now retrieving city # 154, kyaikkami
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kyaikkami&units=imperial
    Now retrieving city # 155, constantine
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=constantine&units=imperial
    Now retrieving city # 156, amderma
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=amderma&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 157, olavarria
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=olavarria&units=imperial
    Now retrieving city # 158, kasangulu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kasangulu&units=imperial
    Now retrieving city # 159, san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san cristobal&units=imperial
    Now retrieving city # 160, beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=beringovskiy&units=imperial
    Now retrieving city # 161, malia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=malia&units=imperial
    Now retrieving city # 162, vallenar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vallenar&units=imperial
    Now retrieving city # 163, padang
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=padang&units=imperial
    Now retrieving city # 164, solaro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=solaro&units=imperial
    Now retrieving city # 165, saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saskylakh&units=imperial
    Now retrieving city # 166, makarov
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=makarov&units=imperial
    Now retrieving city # 167, bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bredasdorp&units=imperial
    Now retrieving city # 168, bousse
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bousse&units=imperial
    Now retrieving city # 169, northam
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=northam&units=imperial
    Now retrieving city # 170, zhigansk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=zhigansk&units=imperial
    Now retrieving city # 171, beyneu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=beyneu&units=imperial
    Now retrieving city # 172, chipinge
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chipinge&units=imperial
    Now retrieving city # 173, georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=georgetown&units=imperial
    Now retrieving city # 174, hobart
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hobart&units=imperial
    Now retrieving city # 175, touros
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=touros&units=imperial
    Now retrieving city # 176, misratah
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=misratah&units=imperial
    Now retrieving city # 177, carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=carnarvon&units=imperial
    Now retrieving city # 178, scottsbluff
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=scottsbluff&units=imperial
    Now retrieving city # 179, santa rosa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=santa rosa&units=imperial
    Now retrieving city # 180, jeremie
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jeremie&units=imperial
    Now retrieving city # 181, mikhaylovka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mikhaylovka&units=imperial
    Now retrieving city # 182, finschhafen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=finschhafen&units=imperial
    Now retrieving city # 183, zharkovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=zharkovskiy&units=imperial
    Now retrieving city # 184, saint george
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saint george&units=imperial
    Now retrieving city # 185, lavumisa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lavumisa&units=imperial
    Now retrieving city # 186, husavik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=husavik&units=imperial
    Now retrieving city # 187, rio gallegos
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rio gallegos&units=imperial
    Now retrieving city # 188, khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=khatanga&units=imperial
    Now retrieving city # 189, salinopolis
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=salinopolis&units=imperial
    Now retrieving city # 190, ayan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ayan&units=imperial
    Now retrieving city # 191, victoria
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=victoria&units=imperial
    Now retrieving city # 192, kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kaeo&units=imperial
    Now retrieving city # 193, lixourion
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lixourion&units=imperial
    Now retrieving city # 194, saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saint-philippe&units=imperial
    Now retrieving city # 195, kungalv
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kungalv&units=imperial
    Now retrieving city # 196, sainte-suzanne
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sainte-suzanne&units=imperial
    Now retrieving city # 197, kano
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kano&units=imperial
    Now retrieving city # 198, ust-kulom
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ust-kulom&units=imperial
    Now retrieving city # 199, wladyslawowo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=wladyslawowo&units=imperial
    Now retrieving city # 200, boffa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=boffa&units=imperial
    Now retrieving city # 201, puerto ayacucho
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=puerto ayacucho&units=imperial
    Now retrieving city # 202, taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=taolanaro&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 203, brae
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=brae&units=imperial
    Now retrieving city # 204, ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ribeira grande&units=imperial
    Now retrieving city # 205, krasnoselkup
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=krasnoselkup&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 206, hammerfest
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hammerfest&units=imperial
    Now retrieving city # 207, oranjemund
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=oranjemund&units=imperial
    Now retrieving city # 208, rome
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rome&units=imperial
    Now retrieving city # 209, dharchula
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dharchula&units=imperial
    Now retrieving city # 210, leh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=leh&units=imperial
    Now retrieving city # 211, kloulklubed
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kloulklubed&units=imperial
    Now retrieving city # 212, isangel
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=isangel&units=imperial
    Now retrieving city # 213, grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=grindavik&units=imperial
    Now retrieving city # 214, huarmey
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=huarmey&units=imperial
    Now retrieving city # 215, thompson
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=thompson&units=imperial
    Now retrieving city # 216, sawakin
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sawakin&units=imperial
    Now retrieving city # 217, saint-joseph
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saint-joseph&units=imperial
    Now retrieving city # 218, alihe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=alihe&units=imperial
    Now retrieving city # 219, ambilobe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ambilobe&units=imperial
    Now retrieving city # 220, wajir
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=wajir&units=imperial
    Now retrieving city # 221, terra nova
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=terra nova&units=imperial
    Now retrieving city # 222, mana
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mana&units=imperial
    Now retrieving city # 223, aripuana
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aripuana&units=imperial
    Now retrieving city # 224, adrar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=adrar&units=imperial
    Now retrieving city # 225, illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=illoqqortoormiut&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 226, polunochnoye
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=polunochnoye&units=imperial
    Now retrieving city # 227, braganca
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=braganca&units=imperial
    Now retrieving city # 228, manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=manokwari&units=imperial
    Now retrieving city # 229, olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=olafsvik&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 230, fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=fairbanks&units=imperial
    Now retrieving city # 231, necochea
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=necochea&units=imperial
    Now retrieving city # 232, sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sao joao da barra&units=imperial
    Now retrieving city # 233, sobolevo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sobolevo&units=imperial
    Now retrieving city # 234, kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kavaratti&units=imperial
    Now retrieving city # 235, fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=fortuna&units=imperial
    Now retrieving city # 236, san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san patricio&units=imperial
    Now retrieving city # 237, shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shimoda&units=imperial
    Now retrieving city # 238, yamada
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yamada&units=imperial
    Now retrieving city # 239, srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=srednekolymsk&units=imperial
    Now retrieving city # 240, iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=iqaluit&units=imperial
    Now retrieving city # 241, palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=palabuhanratu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 242, hornepayne
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hornepayne&units=imperial
    Now retrieving city # 243, vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vila franca do campo&units=imperial
    Now retrieving city # 244, port hedland
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port hedland&units=imperial
    Now retrieving city # 245, ksenyevka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ksenyevka&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 246, riyadh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=riyadh&units=imperial
    Now retrieving city # 247, itaituba
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=itaituba&units=imperial
    Now retrieving city # 248, kulhudhuffushi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kulhudhuffushi&units=imperial
    Now retrieving city # 249, marv dasht
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=marv dasht&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 250, forrest city
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=forrest city&units=imperial
    Now retrieving city # 251, sibu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sibu&units=imperial
    Now retrieving city # 252, norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=norman wells&units=imperial
    Now retrieving city # 253, nakapiripirit
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nakapiripirit&units=imperial
    Now retrieving city # 254, avera
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=avera&units=imperial
    Now retrieving city # 255, honggang
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=honggang&units=imperial
    Now retrieving city # 256, stargard szczecinski
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=stargard szczecinski&units=imperial
    Now retrieving city # 257, ulaangom
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ulaangom&units=imperial
    Now retrieving city # 258, koslan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=koslan&units=imperial
    Now retrieving city # 259, dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dunedin&units=imperial
    Now retrieving city # 260, ayorou
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ayorou&units=imperial
    Now retrieving city # 261, havelock
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=havelock&units=imperial
    Now retrieving city # 262, mansion
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mansion&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 263, coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=coihaique&units=imperial
    Now retrieving city # 264, hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hithadhoo&units=imperial
    Now retrieving city # 265, pietersburg
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pietersburg&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 266, nadadores
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nadadores&units=imperial
    Now retrieving city # 267, wangqing
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=wangqing&units=imperial
    Now retrieving city # 268, muroto
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=muroto&units=imperial
    Now retrieving city # 269, oksfjord
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=oksfjord&units=imperial
    Now retrieving city # 270, camacha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=camacha&units=imperial
    Now retrieving city # 271, tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tessalit&units=imperial
    Now retrieving city # 272, shelburne
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shelburne&units=imperial
    Now retrieving city # 273, warqla
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=warqla&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 274, liberty
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=liberty&units=imperial
    Now retrieving city # 275, bougouni
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bougouni&units=imperial
    Now retrieving city # 276, stephenville
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=stephenville&units=imperial
    Now retrieving city # 277, aksu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aksu&units=imperial
    Now retrieving city # 278, yeppoon
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yeppoon&units=imperial
    Now retrieving city # 279, mangrol
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mangrol&units=imperial
    Now retrieving city # 280, seoul
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=seoul&units=imperial
    Now retrieving city # 281, kahului
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kahului&units=imperial
    Now retrieving city # 282, belyy yar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=belyy yar&units=imperial
    Now retrieving city # 283, kazalinsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kazalinsk&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 284, morrinhos
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=morrinhos&units=imperial
    Now retrieving city # 285, tomigusuku
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tomigusuku&units=imperial
    Now retrieving city # 286, egvekinot
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=egvekinot&units=imperial
    Now retrieving city # 287, whitehorse
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=whitehorse&units=imperial
    Now retrieving city # 288, poya
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=poya&units=imperial
    Now retrieving city # 289, buchanan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=buchanan&units=imperial
    Now retrieving city # 290, neuquen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=neuquen&units=imperial
    Now retrieving city # 291, ostersund
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ostersund&units=imperial
    Now retrieving city # 292, olpad
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=olpad&units=imperial
    Now retrieving city # 293, malumfashi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=malumfashi&units=imperial
    Now retrieving city # 294, nachingwea
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nachingwea&units=imperial
    Now retrieving city # 295, poum
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=poum&units=imperial
    Now retrieving city # 296, eureka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=eureka&units=imperial
    Now retrieving city # 297, sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sao filipe&units=imperial
    Now retrieving city # 298, monrovia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=monrovia&units=imperial
    Now retrieving city # 299, tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tiksi&units=imperial
    Now retrieving city # 300, coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=coahuayana&units=imperial
    Now retrieving city # 301, peniche
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=peniche&units=imperial
    Now retrieving city # 302, lahat
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lahat&units=imperial
    Now retrieving city # 303, tiarei
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tiarei&units=imperial
    Now retrieving city # 304, perth
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=perth&units=imperial
    Now retrieving city # 305, corrientes
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=corrientes&units=imperial
    Now retrieving city # 306, sergeyevka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sergeyevka&units=imperial
    Now retrieving city # 307, ongandjera
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ongandjera&units=imperial
    Now retrieving city # 308, meyungs
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=meyungs&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 309, barry
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=barry&units=imperial
    Now retrieving city # 310, wukari
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=wukari&units=imperial
    Now retrieving city # 311, ketchikan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ketchikan&units=imperial
    Now retrieving city # 312, portland
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=portland&units=imperial
    Now retrieving city # 313, collie
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=collie&units=imperial
    Now retrieving city # 314, san pedro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san pedro&units=imperial
    Now retrieving city # 315, chapais
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chapais&units=imperial
    Now retrieving city # 316, hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hambantota&units=imperial
    Now retrieving city # 317, carauari
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=carauari&units=imperial
    Now retrieving city # 318, pimenta bueno
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pimenta bueno&units=imperial
    Now retrieving city # 319, buala
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=buala&units=imperial
    Now retrieving city # 320, henties bay
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=henties bay&units=imperial
    Now retrieving city # 321, lopukhiv
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lopukhiv&units=imperial
    Now retrieving city # 322, khandyga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=khandyga&units=imperial
    Now retrieving city # 323, nabire
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nabire&units=imperial
    Now retrieving city # 324, dingle
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dingle&units=imperial
    Now retrieving city # 325, biak
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=biak&units=imperial
    Now retrieving city # 326, maridi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=maridi&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 327, akola
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=akola&units=imperial
    Now retrieving city # 328, port blair
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port blair&units=imperial
    Now retrieving city # 329, westport
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=westport&units=imperial
    Now retrieving city # 330, victor harbor
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=victor harbor&units=imperial
    Now retrieving city # 331, usinsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=usinsk&units=imperial
    Now retrieving city # 332, berdigestyakh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=berdigestyakh&units=imperial
    Now retrieving city # 333, shinjo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shinjo&units=imperial
    Now retrieving city # 334, abashiri
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=abashiri&units=imperial
    Now retrieving city # 335, pozo colorado
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pozo colorado&units=imperial
    Now retrieving city # 336, mao
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mao&units=imperial
    Now retrieving city # 337, vanimo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vanimo&units=imperial
    Now retrieving city # 338, inta
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=inta&units=imperial
    Now retrieving city # 339, dalby
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dalby&units=imperial
    Now retrieving city # 340, cockburn town
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cockburn town&units=imperial
    Now retrieving city # 341, warrenton
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=warrenton&units=imperial
    Now retrieving city # 342, cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cabo san lucas&units=imperial
    Now retrieving city # 343, worland
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=worland&units=imperial
    Now retrieving city # 344, yar-sale
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yar-sale&units=imperial
    Now retrieving city # 345, kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kaitangata&units=imperial
    Now retrieving city # 346, birao
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=birao&units=imperial
    Now retrieving city # 347, haines junction
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=haines junction&units=imperial
    Now retrieving city # 348, port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port lincoln&units=imperial
    Now retrieving city # 349, ambon
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ambon&units=imperial
    Now retrieving city # 350, samalaeulu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=samalaeulu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 351, patroha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=patroha&units=imperial
    Now retrieving city # 352, estelle
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=estelle&units=imperial
    Now retrieving city # 353, port moresby
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port moresby&units=imperial
    Now retrieving city # 354, inirida
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=inirida&units=imperial
    Now retrieving city # 355, breves
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=breves&units=imperial
    Now retrieving city # 356, nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nizhneyansk&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 357, limoncito
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=limoncito&units=imperial
    Now retrieving city # 358, eucaliptus
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=eucaliptus&units=imperial
    Now retrieving city # 359, nemuro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nemuro&units=imperial
    Now retrieving city # 360, birin
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=birin&units=imperial
    Now retrieving city # 361, lata
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lata&units=imperial
    Now retrieving city # 362, dekar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dekar&units=imperial
    Now retrieving city # 363, naraini
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=naraini&units=imperial
    Now retrieving city # 364, pierre
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pierre&units=imperial
    Now retrieving city # 365, kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kodiak&units=imperial
    Now retrieving city # 366, ust-kachka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ust-kachka&units=imperial
    Now retrieving city # 367, havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=havoysund&units=imperial
    Now retrieving city # 368, vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vaitupu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 369, zagare
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=zagare&units=imperial
    Now retrieving city # 370, terra santa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=terra santa&units=imperial
    Now retrieving city # 371, mazagao
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mazagao&units=imperial
    Now retrieving city # 372, dapdap
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dapdap&units=imperial
    Now retrieving city # 373, billingham
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=billingham&units=imperial
    Now retrieving city # 374, karlskrona
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=karlskrona&units=imperial
    Now retrieving city # 375, tome-acu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tome-acu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 376, rocha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rocha&units=imperial
    Now retrieving city # 377, rawannawi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rawannawi&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 378, lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lavrentiya&units=imperial
    Now retrieving city # 379, roblin
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=roblin&units=imperial
    Now retrieving city # 380, puerto colombia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=puerto colombia&units=imperial
    Now retrieving city # 381, doha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=doha&units=imperial
    Now retrieving city # 382, vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vestmannaeyjar&units=imperial
    Now retrieving city # 383, antipovka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=antipovka&units=imperial
    Now retrieving city # 384, taksimo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=taksimo&units=imperial
    Now retrieving city # 385, elat
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=elat&units=imperial
    Now retrieving city # 386, batemans bay
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=batemans bay&units=imperial
    Now retrieving city # 387, arlit
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=arlit&units=imperial
    Now retrieving city # 388, emporia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=emporia&units=imperial
    Now retrieving city # 389, safwah
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=safwah&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 390, sabang
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sabang&units=imperial
    Now retrieving city # 391, san jose de guanipa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san jose de guanipa&units=imperial
    Now retrieving city # 392, winchester
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=winchester&units=imperial
    Now retrieving city # 393, kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kalmunai&units=imperial
    Now retrieving city # 394, bellevue
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bellevue&units=imperial
    Now retrieving city # 395, esperance
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=esperance&units=imperial
    Now retrieving city # 396, nelson bay
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nelson bay&units=imperial
    Now retrieving city # 397, tutoia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tutoia&units=imperial
    Now retrieving city # 398, gayny
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=gayny&units=imperial
    Now retrieving city # 399, miramar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=miramar&units=imperial
    Now retrieving city # 400, panama city
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=panama city&units=imperial
    Now retrieving city # 401, tazovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tazovskiy&units=imperial
    Now retrieving city # 402, turukhansk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=turukhansk&units=imperial
    Now retrieving city # 403, turka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=turka&units=imperial
    Now retrieving city # 404, takestan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=takestan&units=imperial
    Now retrieving city # 405, chauk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chauk&units=imperial
    Now retrieving city # 406, owando
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=owando&units=imperial
    Now retrieving city # 407, verkhniy avzyan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=verkhniy avzyan&units=imperial
    Now retrieving city # 408, camacupa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=camacupa&units=imperial
    Now retrieving city # 409, terney
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=terney&units=imperial
    Now retrieving city # 410, deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=deputatskiy&units=imperial
    Now retrieving city # 411, nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nouadhibou&units=imperial
    Now retrieving city # 412, klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=klaksvik&units=imperial
    Now retrieving city # 413, kaohsiung
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kaohsiung&units=imperial
    Now retrieving city # 414, rovaniemi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rovaniemi&units=imperial
    Now retrieving city # 415, mundo nuevo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mundo nuevo&units=imperial
    Now retrieving city # 416, vardo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vardo&units=imperial
    Now retrieving city # 417, sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sisimiut&units=imperial
    Now retrieving city # 418, sakakah
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sakakah&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 419, bouake
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bouake&units=imperial
    Now retrieving city # 420, bagotville
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bagotville&units=imperial
    Now retrieving city # 421, shwebo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shwebo&units=imperial
    Now retrieving city # 422, bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bandarbeyla&units=imperial
    Now retrieving city # 423, evanston
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=evanston&units=imperial
    Now retrieving city # 424, honningsvag
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=honningsvag&units=imperial
    Now retrieving city # 425, ampanihy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ampanihy&units=imperial
    Now retrieving city # 426, inhambane
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=inhambane&units=imperial
    Now retrieving city # 427, lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lagoa&units=imperial
    Now retrieving city # 428, acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=acapulco&units=imperial
    Now retrieving city # 429, burgeo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=burgeo&units=imperial
    Now retrieving city # 430, livramento
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=livramento&units=imperial
    Now retrieving city # 431, miri
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=miri&units=imperial
    Now retrieving city # 432, sorland
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sorland&units=imperial
    Now retrieving city # 433, snyder
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=snyder&units=imperial
    Now retrieving city # 434, makakilo city
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=makakilo city&units=imperial
    Now retrieving city # 435, jinchang
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jinchang&units=imperial
    Now retrieving city # 436, urdzhar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=urdzhar&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 437, otane
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=otane&units=imperial
    Now retrieving city # 438, santa luzia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=santa luzia&units=imperial
    Now retrieving city # 439, rosia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rosia&units=imperial
    Now retrieving city # 440, azimur
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=azimur&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 441, carmen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=carmen&units=imperial
    Now retrieving city # 442, tankhoy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tankhoy&units=imperial
    Now retrieving city # 443, scarborough
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=scarborough&units=imperial
    Now retrieving city # 444, jhanjharpur
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jhanjharpur&units=imperial
    Now retrieving city # 445, watsa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=watsa&units=imperial
    Now retrieving city # 446, alofi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=alofi&units=imperial
    Now retrieving city # 447, hami
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hami&units=imperial
    Now retrieving city # 448, saraland
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saraland&units=imperial
    Now retrieving city # 449, kualakapuas
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kualakapuas&units=imperial
    Now retrieving city # 450, rincon
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rincon&units=imperial
    Now retrieving city # 451, aksarka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aksarka&units=imperial
    Now retrieving city # 452, muisne
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=muisne&units=imperial
    Now retrieving city # 453, gao
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=gao&units=imperial
    Now retrieving city # 454, bonito
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bonito&units=imperial
    Now retrieving city # 455, banska bystrica
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=banska bystrica&units=imperial
    Now retrieving city # 456, oyama
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=oyama&units=imperial
    Now retrieving city # 457, camana
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=camana&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 458, tunxi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tunxi&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 459, naze
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=naze&units=imperial
    Now retrieving city # 460, kharitonovo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kharitonovo&units=imperial
    Now retrieving city # 461, guozhen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=guozhen&units=imperial
    Now retrieving city # 462, kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kavieng&units=imperial
    Now retrieving city # 463, labrea
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=labrea&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 464, alpinopolis
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=alpinopolis&units=imperial
    Now retrieving city # 465, taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=taoudenni&units=imperial
    Now retrieving city # 466, corumba
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=corumba&units=imperial
    Now retrieving city # 467, maniitsoq
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=maniitsoq&units=imperial
    Now retrieving city # 468, redlands
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=redlands&units=imperial
    Now retrieving city # 469, bubaque
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bubaque&units=imperial
    Now retrieving city # 470, kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kangaatsiaq&units=imperial
    Now retrieving city # 471, okhotsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=okhotsk&units=imperial
    Now retrieving city # 472, bedele
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bedele&units=imperial
    Now retrieving city # 473, kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kruisfontein&units=imperial
    Now retrieving city # 474, mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mount gambier&units=imperial
    Now retrieving city # 475, mumford
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mumford&units=imperial
    Now retrieving city # 476, half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=half moon bay&units=imperial
    Now retrieving city # 477, storforshei
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=storforshei&units=imperial
    Now retrieving city # 478, moree
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=moree&units=imperial
    Now retrieving city # 479, mudgee
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mudgee&units=imperial
    Now retrieving city # 480, aquiraz
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aquiraz&units=imperial
    Now retrieving city # 481, danielskuil
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=danielskuil&units=imperial
    Now retrieving city # 482, berlevag
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=berlevag&units=imperial
    Now retrieving city # 483, biga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=biga&units=imperial
    Now retrieving city # 484, sitka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sitka&units=imperial
    Now retrieving city # 485, aflu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aflu&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 486, tharad
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tharad&units=imperial
    Now retrieving city # 487, pizarro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pizarro&units=imperial
    Now retrieving city # 488, goderich
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=goderich&units=imperial
    Now retrieving city # 489, louisbourg
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=louisbourg&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 490, supaul
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=supaul&units=imperial
    Now retrieving city # 491, sioux lookout
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sioux lookout&units=imperial
    Now retrieving city # 492, chinsali
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chinsali&units=imperial
    Now retrieving city # 493, gizo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=gizo&units=imperial
    Now retrieving city # 494, jalu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jalu&units=imperial
    Now retrieving city # 495, asfi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=asfi&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 496, esso
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=esso&units=imperial
    Now retrieving city # 497, braslav
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=braslav&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 498, rambha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rambha&units=imperial
    Now retrieving city # 499, amboasary
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=amboasary&units=imperial
    Now retrieving city # 500, saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saleaula&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 501, yanam
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yanam&units=imperial
    Now retrieving city # 502, toliary
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=toliary&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 503, yavaros
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yavaros&units=imperial
    Now retrieving city # 504, kushiro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kushiro&units=imperial
    Now retrieving city # 505, san luis
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san luis&units=imperial
    Now retrieving city # 506, lufilufi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lufilufi&units=imperial
    Now retrieving city # 507, harper
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=harper&units=imperial
    Now retrieving city # 508, mahajanga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mahajanga&units=imperial
    Now retrieving city # 509, acari
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=acari&units=imperial
    Now retrieving city # 510, anadyr
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=anadyr&units=imperial
    Now retrieving city # 511, tuggurt
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tuggurt&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 512, sur
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sur&units=imperial
    Now retrieving city # 513, tarudant
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tarudant&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 514, hede
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hede&units=imperial
    Now retrieving city # 515, haines city
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=haines city&units=imperial
    Now retrieving city # 516, upata
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=upata&units=imperial
    Now retrieving city # 517, flin flon
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=flin flon&units=imperial
    Now retrieving city # 518, bandar-e lengeh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bandar-e lengeh&units=imperial
    Now retrieving city # 519, saint-augustin
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saint-augustin&units=imperial
    Now retrieving city # 520, tigil
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tigil&units=imperial
    Now retrieving city # 521, kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kurilsk&units=imperial
    Now retrieving city # 522, lima
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lima&units=imperial
    Now retrieving city # 523, belaya gora
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=belaya gora&units=imperial
    Now retrieving city # 524, japura
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=japura&units=imperial
    Now retrieving city # 525, mopipi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mopipi&units=imperial
    Now retrieving city # 526, barra patuca
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=barra patuca&units=imperial
    Now retrieving city # 527, bull savanna
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bull savanna&units=imperial
    Now retrieving city # 528, mathbaria
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mathbaria&units=imperial
    Now retrieving city # 529, kananga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kananga&units=imperial
    Now retrieving city # 530, zhezkazgan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=zhezkazgan&units=imperial
    Now retrieving city # 531, jonkoping
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jonkoping&units=imperial
    Now retrieving city # 532, lunel
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lunel&units=imperial
    Now retrieving city # 533, kiama
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kiama&units=imperial
    Now retrieving city # 534, christchurch
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=christchurch&units=imperial
    Now retrieving city # 535, tumannyy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tumannyy&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 536, petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=petropavlovsk-kamchatskiy&units=imperial
    Now retrieving city # 537, taltal
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=taltal&units=imperial
    Now retrieving city # 538, puerto escondido
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=puerto escondido&units=imperial
    Now retrieving city # 539, santa cruz
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=santa cruz&units=imperial
    Now retrieving city # 540, abu samrah
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=abu samrah&units=imperial
    Now retrieving city # 541, sofiysk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sofiysk&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 542, mogzon
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mogzon&units=imperial
    Now retrieving city # 543, kibala
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kibala&units=imperial
    Now retrieving city # 544, revelstoke
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=revelstoke&units=imperial
    Now retrieving city # 545, weatherford
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=weatherford&units=imperial
    Now retrieving city # 546, kodinsk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kodinsk&units=imperial
    Now retrieving city # 547, waipawa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=waipawa&units=imperial
    Now retrieving city # 548, sibolga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sibolga&units=imperial
    Now retrieving city # 549, sanok
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sanok&units=imperial
    Now retrieving city # 550, salym
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=salym&units=imperial
    Now retrieving city # 551, ternate
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ternate&units=imperial
    Now retrieving city # 552, sumbe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sumbe&units=imperial
    Now retrieving city # 553, orlik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=orlik&units=imperial
    Now retrieving city # 554, tarata
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tarata&units=imperial
    Now retrieving city # 555, araouane
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=araouane&units=imperial
    Now retrieving city # 556, buraydah
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=buraydah&units=imperial
    Now retrieving city # 557, nijar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nijar&units=imperial
    Now retrieving city # 558, soyo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=soyo&units=imperial
    Now retrieving city # 559, kununurra
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kununurra&units=imperial
    Now retrieving city # 560, prince rupert
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=prince rupert&units=imperial
    Now retrieving city # 561, moussoro
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=moussoro&units=imperial
    Now retrieving city # 562, sim
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sim&units=imperial
    Now retrieving city # 563, werda
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=werda&units=imperial
    Now retrieving city # 564, mendahara
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mendahara&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 565, aliwal north
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aliwal north&units=imperial
    Now retrieving city # 566, igarka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=igarka&units=imperial
    Now retrieving city # 567, palana
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=palana&units=imperial
    Now retrieving city # 568, santa fe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=santa fe&units=imperial
    Now retrieving city # 569, ngukurr
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ngukurr&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 570, kedrovyy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kedrovyy&units=imperial
    Now retrieving city # 571, vila velha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=vila velha&units=imperial
    Now retrieving city # 572, beruwala
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=beruwala&units=imperial
    Now retrieving city # 573, cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cayenne&units=imperial
    Now retrieving city # 574, kashi
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kashi&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 575, pevek
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pevek&units=imperial
    Now retrieving city # 576, shiyan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shiyan&units=imperial
    Now retrieving city # 577, kirensk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kirensk&units=imperial
    Now retrieving city # 578, kendari
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kendari&units=imperial
    Now retrieving city # 579, eidsvag
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=eidsvag&units=imperial
    Now retrieving city # 580, meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=meulaboh&units=imperial
    Now retrieving city # 581, honiara
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=honiara&units=imperial
    Now retrieving city # 582, shanghai
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shanghai&units=imperial
    Now retrieving city # 583, segovia
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=segovia&units=imperial
    Now retrieving city # 584, atlatlahuca
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=atlatlahuca&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 585, cardenas
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cardenas&units=imperial
    Now retrieving city # 586, pilar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pilar&units=imperial
    Now retrieving city # 587, alyangula
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=alyangula&units=imperial
    Now retrieving city # 588, kinshasa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kinshasa&units=imperial
    Now retrieving city # 589, kem
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kem&units=imperial
    Now retrieving city # 590, listvyanka
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=listvyanka&units=imperial
    Now retrieving city # 591, havre-saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=havre-saint-pierre&units=imperial
    Now retrieving city # 592, subate
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=subate&units=imperial
    Now retrieving city # 593, geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=geraldton&units=imperial
    Now retrieving city # 594, fernandopolis
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=fernandopolis&units=imperial
    Now retrieving city # 595, urambo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=urambo&units=imperial
    Now retrieving city # 596, rabo de peixe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rabo de peixe&units=imperial
    Now retrieving city # 597, la rioja
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=la rioja&units=imperial
    Now retrieving city # 598, voskresenskoye
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=voskresenskoye&units=imperial
    Now retrieving city # 599, gravelbourg
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=gravelbourg&units=imperial
    Now retrieving city # 600, zhucheng
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=zhucheng&units=imperial
    Now retrieving city # 601, divnomorskoye
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=divnomorskoye&units=imperial
    Now retrieving city # 602, homer
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=homer&units=imperial
    Now retrieving city # 603, valle de allende
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=valle de allende&units=imperial
    Now retrieving city # 604, koszalin
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=koszalin&units=imperial
    Now retrieving city # 605, lasa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lasa&units=imperial
    Now retrieving city # 606, hervey bay
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hervey bay&units=imperial
    Now retrieving city # 607, papara
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=papara&units=imperial
    Now retrieving city # 608, shupiyan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=shupiyan&units=imperial
    Now retrieving city # 609, mehamn
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mehamn&units=imperial
    Now retrieving city # 610, smithers
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=smithers&units=imperial
    Now retrieving city # 611, mouila
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mouila&units=imperial
    Now retrieving city # 612, conceicao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=conceicao da barra&units=imperial
    Now retrieving city # 613, chagda
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=chagda&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 614, pultusk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pultusk&units=imperial
    Now retrieving city # 615, mogadishu
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mogadishu&units=imperial
    Now retrieving city # 616, mansa
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mansa&units=imperial
    Now retrieving city # 617, samandag
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=samandag&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 618, marcona
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=marcona&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 619, noumea
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=noumea&units=imperial
    Now retrieving city # 620, mitsamiouli
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mitsamiouli&units=imperial
    Now retrieving city # 621, qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=qasigiannguit&units=imperial
    Now retrieving city # 622, healdsburg
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=healdsburg&units=imperial
    Now retrieving city # 623, kizukuri
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kizukuri&units=imperial
    Now retrieving city # 624, viligili
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=viligili&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 625, attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=attawapiskat&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 626, teahupoo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=teahupoo&units=imperial
    Now retrieving city # 627, hovd
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hovd&units=imperial
    Now retrieving city # 628, yelizovo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=yelizovo&units=imperial
    Now retrieving city # 629, san jose
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san jose&units=imperial
    Now retrieving city # 630, clarence town
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=clarence town&units=imperial
    Now retrieving city # 631, hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=hobyo&units=imperial
    Now retrieving city # 632, port-cartier
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=port-cartier&units=imperial
    Now retrieving city # 633, aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=aklavik&units=imperial
    Now retrieving city # 634, lazarev
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=lazarev&units=imperial
    Now retrieving city # 635, candawaga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=candawaga&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 636, moron
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=moron&units=imperial
    Now retrieving city # 637, mut
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mut&units=imperial
    Now retrieving city # 638, obidos
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=obidos&units=imperial
    Now retrieving city # 639, damghan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=damghan&units=imperial
    Now retrieving city # 640, voyvozh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=voyvozh&units=imperial
    Now retrieving city # 641, ashland
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ashland&units=imperial
    Now retrieving city # 642, strezhevoy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=strezhevoy&units=imperial
    Now retrieving city # 643, maumere
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=maumere&units=imperial
    Now retrieving city # 644, solnechnyy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=solnechnyy&units=imperial
    Now retrieving city # 645, san lawrenz
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san lawrenz&units=imperial
    Now retrieving city # 646, labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=labuhan&units=imperial
    Now retrieving city # 647, marsa matruh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=marsa matruh&units=imperial
    Now retrieving city # 648, bosobolo
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=bosobolo&units=imperial
    Now retrieving city # 649, baishishan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=baishishan&units=imperial
    Now retrieving city # 650, sinjar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sinjar&units=imperial
    Now retrieving city # 651, nuqui
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nuqui&units=imperial
    Now retrieving city # 652, ballangen
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ballangen&units=imperial
    Now retrieving city # 653, kiunga
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kiunga&units=imperial
    Now retrieving city # 654, panjab
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=panjab&units=imperial
    Now retrieving city # 655, san vicente
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=san vicente&units=imperial
    Now retrieving city # 656, pecos
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pecos&units=imperial
    Now retrieving city # 657, muscat
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=muscat&units=imperial
    Now retrieving city # 658, eyl
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=eyl&units=imperial
    Now retrieving city # 659, flinders
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=flinders&units=imperial
    Now retrieving city # 660, ginir
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ginir&units=imperial
    Now retrieving city # 661, sroda wielkopolska
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sroda wielkopolska&units=imperial
    Now retrieving city # 662, saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saint-pierre&units=imperial
    Now retrieving city # 663, raahe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=raahe&units=imperial
    Now retrieving city # 664, ulundurpettai
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ulundurpettai&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 665, cherdyn
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cherdyn&units=imperial
    Now retrieving city # 666, margate
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=margate&units=imperial
    Now retrieving city # 667, talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=talnakh&units=imperial
    Now retrieving city # 668, berezovyy
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=berezovyy&units=imperial
    Now retrieving city # 669, la palma
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=la palma&units=imperial
    Now retrieving city # 670, constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=constitucion&units=imperial
    Now retrieving city # 671, cocal
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cocal&units=imperial
    Now retrieving city # 672, dalnerechensk
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=dalnerechensk&units=imperial
    Now retrieving city # 673, kambove
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=kambove&units=imperial
    Now retrieving city # 674, wuda
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=wuda&units=imperial
    Now retrieving city # 675, batagay-alyta
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=batagay-alyta&units=imperial
    Now retrieving city # 676, quatre cocos
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=quatre cocos&units=imperial
    Now retrieving city # 677, ushtobe
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ushtobe&units=imperial
    Now retrieving city # 678, sinnamary
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=sinnamary&units=imperial
    Now retrieving city # 679, ornskoldsvik
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=ornskoldsvik&units=imperial
    Now retrieving city # 680, gomel
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=gomel&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 681, mehran
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=mehran&units=imperial
    Now retrieving city # 682, makali
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=makali&units=imperial
    Now retrieving city # 683, alirajpur
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=alirajpur&units=imperial
    Now retrieving city # 684, big spring
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=big spring&units=imperial
    Now retrieving city # 685, saint-francois
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=saint-francois&units=imperial
    Now retrieving city # 686, formosa do rio preto
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=formosa do rio preto&units=imperial
    Now retrieving city # 687, haibowan
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=haibowan&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 688, pisco
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=pisco&units=imperial
    Now retrieving city # 689, banjar
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=banjar&units=imperial
    Now retrieving city # 690, jinxiang
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=jinxiang&units=imperial
    Now retrieving city # 691, rawson
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rawson&units=imperial
    Now retrieving city # 692, tucuma
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tucuma&units=imperial
    Error with city weather data. Skipping
    Now retrieving city # 693, goundam
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=goundam&units=imperial
    Now retrieving city # 694, cap malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=cap malheureux&units=imperial
    Now retrieving city # 695, horsham
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=horsham&units=imperial
    Now retrieving city # 696, rantauprapat
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=rantauprapat&units=imperial
    Now retrieving city # 697, tura
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=tura&units=imperial
    Now retrieving city # 698, karratha
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=karratha&units=imperial
    Now retrieving city # 699, nantucket
    http://api.openweathermap.org/data/2.5/weather?appid=api_key&q=nantucket&units=imperial



```python
# Save dataframe as a csv
weather_df.to_csv("output_weather_data.csv")
```


```python
# Remove rows with missing weather data
clean_weather_df = weather_df.dropna(axis=0,how="any")
```


```python
# Latitude vs Temperature Plot
plt.figure(figsize=(20,10))
plt.scatter(clean_weather_df["latitude"],clean_weather_df["temperature"], marker="o")
plt.xlabel("Latitude",fontsize=20)
plt.ylabel("Maximum Temperature (F)",fontsize=20)
plt.title("City Latitude vs. Maximum Temperature (03/20/18)",fontsize=22)
plt.xlim([-100,100])
plt.ylim([-100,150])
plt.grid(True)

# Save the figure
plt.savefig("output_latitude_vs_temperature.png")

plt.show()
```


![png](output_7_0.png)



```python
# Latitude vs Humidity Plot
plt.figure(figsize=(20,10))
plt.scatter(clean_weather_df["latitude"],clean_weather_df["humidity"],marker="o")
plt.xlabel("Latitude",fontsize=20)
plt.ylabel("Humidity (%)",fontsize=20)
plt.title("City Latitude vs. Humidity (03/20/18)",fontsize=22)
plt.xlim([-100,100])
plt.ylim([-5,105])
plt.grid(True)

# Save the figure
plt.savefig("output_latitude_vs_humidity.png")

plt.show()
```


![png](output_8_0.png)



```python
# Latitude vs Cloudiness Plot
plt.figure(figsize=(20,10))
plt.scatter(clean_weather_df["latitude"],clean_weather_df["cloudiness"],marker="o")
plt.xlabel("Latitude",fontsize=20)
plt.ylabel("Cloudiness (%)",fontsize=20)
plt.title("City Latitude vs. Cloudiness (03/20/18)",fontsize=22)
plt.xlim([-100,100])
plt.ylim([-5,105])
plt.grid(True)

# Save the figure
plt.savefig("output_latitude_vs_cloudiness.png")

plt.show()
```


![png](output_9_0.png)



```python
# Latitude vs. Wind Speed Plot
plt.figure(figsize=(20,10))
plt.scatter(clean_weather_df["latitude"],clean_weather_df["wind speed"],marker="o")
plt.xlabel("Latitude",fontsize=20)
plt.ylabel("Wind Speed (mph)",fontsize=20)
plt.title("City Latitude vs. Wind Speed (03/20/18)",fontsize=22)
plt.xlim([-100,100])
plt.ylim([-1,35])
plt.grid(True)

# Save the figure
plt.savefig("output_latitude_vs_windspeed.png")

plt.show()
```


![png](output_10_0.png)

