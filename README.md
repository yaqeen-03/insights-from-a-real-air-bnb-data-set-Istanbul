# insights-from-a-real-air-bnb-data-set-Istanbul
python code for insights on airbwb booking of Istanbul we are using calendar add listing csv files
Please find the link for downloading
https://data.insideairbnb.com/turkey/marmara/istanbul/2024-09-28/data/calendar.csv.gz
<img width="1094" alt="Screenshot 1446-07-14 at 9 53 25 AM" src="https://github.com/user-attachments/assets/6717219e-7185-4ccf-8f4e-79ecb498217c" />


```python
import pandas as pd
calendar=pd.read_csv("calendar.csv")
```

# ✅ Questions that you might want to address about the given data

Let`s dive into exciting insights!

# 1. Want to know the number of available and unavailable rooms

```python
calendar.available.value_counts()
```

<img width="301" alt="Screenshot 1446-07-28 at 9 39 51 AM" src="https://github.com/user-attachments/assets/74006dba-f4c9-4c2c-b0b7-11c002ae4bc7" />

f (false) means not available, t(true) means available.

# 2 Purpose: Calculates the percentage of available (t) and unavailable (f) dates.

Explanation:
value_counts(normalize=True) gives proportions.
Multiplying by 100 converts the proportions into percentages

```python
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
# 3 Let`s Count the busiest day! 🚩
Hint: We will be counting the most unavailable days (given by f)

# 4 Plot a bar graph to show availability percentage

```python
import matplotlib.pyplot as plt

availability_percentage.plot(kind='bar', color=['#FADADD', '#F5E6CC']) 
plt.title('Availability Percentages', fontsize=16)
plt.ylabel('Percentage', fontsize=12)
plt.xlabel('Available (t/f)', fontsize=12)
plt.xticks(fontsize=10)
plt.yticks(fontsize=10)
plt.show()
```
<img width="530" alt="Screenshot 1446-07-28 at 9 52 30 AM" src="https://github.com/user-attachments/assets/b5ea67b1-4d74-42f7-b5a4-11297dc1ab02" />

# 5 Plot the busiest day

```python
busiest_dates.head(10).plot(kind='bar', color='#FADADD')  
plt.title('Top 10 Busiest Dates', fontsize=16)
plt.ylabel('Number of Listings Unavailable', fontsize=12)
plt.xlabel('Date', fontsize=12)
plt.xticks(fontsize=10, rotation=45) 
plt.yticks(fontsize=10)
plt.show()
```
<img width="562" alt="Screenshot 1446-07-28 at 9 54 31 AM" src="https://github.com/user-attachments/assets/e9715555-2a43-4114-8da1-5b9b433baac0" />


# 📄 Download listings dataset of Hawaii from
# https://data.insideairbnb.com/turkey/marmara/istanbul/2024-09-28/visualisations/listings.csv

```python
import pandas as pd
listings = pd.read_csv('listings.csv')
listings
```
<img width="714" alt="Screenshot 1446-07-28 at 10 01 22 AM" src="https://github.com/user-attachments/assets/379d9407-7e42-4883-adc5-4d1283746661" />

```python
listings.columns
```
<img width="532" alt="Screenshot 1446-07-28 at 10 02 39 AM" src="https://github.com/user-attachments/assets/db56b149-f7bd-4250-9f23-0c70b5b48b1b" />

# ✅ Room type and price is given seperately
Let`s Combine and visualize them
```python
import matplotlib.pyplot as plt

price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

price_by_room.plot(kind='bar', color='#FADADD')  
plt.title('Average Price by Room Type', fontsize=16)
plt.ylabel('Average Price', fontsize=12)
plt.xlabel('Room Type', fontsize=12)
plt.xticks(fontsize=10)
plt.yticks(fontsize=10)
plt.show()
```
<img width="481" alt="Screenshot 1446-07-28 at 10 04 43 AM" src="https://github.com/user-attachments/assets/55b9b0d8-4c8d-4232-b7b1-b4bca7f070b1" />

# ✅ Which are the top 10 neighborhoods with the most listings?
```python
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

neighborhood_counts.plot(kind='bar', color='#FADADD')  
plt.title('Top 10 Neighborhoods by Listings', fontsize=16)
plt.ylabel('Number of Listings', fontsize=12)
plt.xlabel('Neighborhood', fontsize=12)
plt.xticks(rotation=90, fontsize=10) 
plt.yticks(fontsize=10)
plt.show()
```
<img width="570" alt="Screenshot 1446-07-28 at 10 06 47 AM" src="https://github.com/user-attachments/assets/c31a8c8f-6d26-482f-b741-2171a93c0ce7" />

# ✅ Geographical Distribution of Listings (Price Colored)
```python
import matplotlib.pyplot as plt
import seaborn as sns
```
```python
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="783" alt="Screenshot 1446-07-28 at 10 08 49 AM" src="https://github.com/user-attachments/assets/d7ff15cd-12bc-4940-a513-10523694e8f9" />

# Let us see the listings on a real map
* High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas.Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Istanbul where people tend to stay more often.
Colder Areas (Green/Blue):
* Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available.Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic.
Density Patterns:
* Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like markets, historical sites, or urban centers.
Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Istanbul.

```python
import folium
from folium.plugins import HeatMap
import pandas as pd

istanbul_data = listings[['latitude', 'longitude', 'price']]
m = folium.Map(location=[41.0082, 28.9784], zoom_start=12)

heat_data = [[row['latitude'], row['longitude']] for index, row in istanbul_data.iterrows()]

HeatMap(heat_data).add_to(m)
m.save('istanbul_heatmap.html')
m
```

<img width="794" alt="Screenshot 1446-07-28 at 10 21 35 AM" src="https://github.com/user-attachments/assets/c4de27d5-6800-4cca-8fb8-2a5db1b570c5" />

# 🚨 How do I find location for my city?

Type your city name on google maps
Click on What`s here

<img width="1465" alt="Screenshot 1446-07-28 at 10 23 27 AM" src="https://github.com/user-attachments/assets/95b3ad19-2f40-4191-aebb-f097a182d0a2" />


You will find latitude and longitude at the bottom of screen


<img width="386" alt="Screenshot 1446-07-28 at 10 24 38 AM" src="https://github.com/user-attachments/assets/8fa3e372-59d4-4021-b4ee-ffcd5a735b95" />




