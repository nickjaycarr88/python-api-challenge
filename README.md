# python-api-challenge
---
## Installations
### matplotlib, pandas, numpy, requests, time, scipy, citipy, gmaps.
---
### This project was designed to test my knwoledge of accessing and importing API information, searching through the information to retrieve relevant data and then to post that data to a dataframe. The next phase of the module was to filter the dataframe to match my exact needs and then create a heat and layer map using latitude and longituede coordinates.

---
### Data's true power is its ability to definitively answer questions. So, let's take what you've learned about Python requests, APIs, and JSON traversals to answer a fundamental question: "What is the weather like as we approach the equator?"

### Now, we know what you may be thinking: “That’s obvious. It gets hotter.” But, if pressed for more information, how would you prove that?


---
## WeatherPy

### In this deliverable, you'll create a Python script to visualise the weather of over 500 cities of varying distances from the equator. You'll use the citipy Python library Links to an external site., the OpenWeatherMap API Links to an external site., and your problem-solving skills to create a representative model of weather across cities.

### Calling API and returing specific information
```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
lat_lngs = list(zip(lats, lngs))

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

url = f"http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID={weather_api_key}"

# city = "saldanha"
city_url = url + "&q=" + city.replace(" ","+")

response = requests.get(city_url)
response = response.json()

city_name = []
latitude_list = []
longitude_list = []
max_temp_list = []
humidity_list = []
cloudiness_list = []
wind_speed_list = []
country = []
date_list = []

for city in cities:
    city_url = url + "&q=" + city.replace(" ","+")
    response = requests.get(city_url).json()
    
    try:
        
        
        latitude = response["coord"]["lat"]
        latitude_list.append(latitude)
        
        longitude = response["coord"]["lon"]
        longitude_list.append(longitude)
        
        max_temp = response["main"]["temp_max"]
        max_temp_list.append(max_temp)
        
        humidity = response["main"]["humidity"]
        humidity_list.append(humidity)
        
        cloudiness = response["clouds"]['all']
        cloudiness_list.append(cloudiness)
        
        wind_speed = response["wind"]["speed"]
        wind_speed_list.append(wind_speed)
        
        country_name = response["sys"]["country"]
        country.append(country_name)
        
        date = response["dt"]
        date_list.append(date)
        
        city_iterate = city
        city_name.append(city_iterate)
        
        print(f"Processing Record {cities.index(city)+1} of Set 1 | {city}")
    
    except:
        print("City not found. Skipping...")
        pass
  
   
```
### **Requirement 1: Create Plots to Showcase the Relationship Between Weather Variables and Latitude**
### Latitude v Humidity
![Image Link](https://github.com/nickjaycarr88/python-api-challenge/blob/main/images/lat_humidity.png)


### Latitude v Humidity with linear regression
![Image Link](https://github.com/nickjaycarr88/python-api-challenge/blob/main/images/lat_humidity_linear.png)

---
## VacationPy
### Your main tasks will be to use the Geoapify API and the geoViews Python library and employ your Python skills to create map visualisations.

### A heat map displaying every city in the dataframe
![Image Link](https://github.com/nickjaycarr88/python-api-challenge/blob/main/images/heat_map.png)


### A map with markers displaying information of the hotels with the following conditions: temperature between 22 and 26 degrees, 0 cloudiness and a wind speed of less than 4.5.
![Image Link](https://github.com/nickjaycarr88/python-api-challenge/blob/main/images/markers_on_heat_map.png)

### A few findings from WeatherPy 
1-The wind speed for both northern and southern hemisphere is generally below 20 mph for the whole world. However there is no strong correlation between the latitude and wind speed data. The use of a scatter plot is helpful to show how little correlation there is.

2-As you can probably guess, the humidity decreases the furthere away you are from the equator. Both northern hemisphere- humidity vs latitude and southern hemisphere- humidity vs latitude scatter plots with a linear regression line thoroughly indicate this.

3-In relation to cloudiness v latitude, there is abviously clouds throughout the world. An interesting abservation is that at both the north and south pole there are no clouds present.