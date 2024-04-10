#!/usr/bin/env python
# coding: utf-8

# In[1]:


import requests # module to make an API call
import json
import pandas as pd


# In[2]:


# Collecting USA Hotel Rating Data Using API

# Google Maps API key
api_key = "AIzaSyD1Z-zzP-Klzypvx9JaXYyoveACbbwCugc"

# Cities to collect hotel data
cities = ["Kent, Ohio", "Boston, Massachusetts", "Seattle, Washington"]

def get_hotels(api_key, location):
    # Define the radius (in meters) for the search
    radius = 1000 
    import requests # module to make an API call
import json
import pandas as pd
    # Create a request to the Places API to search for hotels in the given location
    url = "https://maps.googleapis.com/maps/api/place/textsearch/json"
    params = {
        "query": f"hotels in {location}",
        "radius": radius,
        "key": api_key
    }

    # Make the API request
    response = requests.get(url, params=params)

    # Check if the request was successful
    if response.status_code == 200:
        data = response.json() # Parse JSON response
        return data.get("results", []) # Return hotel data
    else:
        print(f"Error: Unable to fetch data for {location}. Status code: {response.status_code}")
        return []

# Create an empty list to store all hotel data
all_hotels = []

# Iterate through the cities and retrieve hotel data
for city in cities:
    print(f"Collecting hotel data for {city}...")
    hotels = get_hotels(api_key, city)
    all_hotels.extend(hotels)


# In[3]:


# Save hotel data to a JSON file
with open("hotel_data.json", "w") as json_file:
    json.dump(all_hotels, json_file, indent=4)

print("Hotel data saved to hotel_data.json.")

# Load the JSON data into a pandas DataFrame
df = pd.DataFrame(all_hotels)

# Save Data as a csv file
# File path
csv_path = r"C:\Users\Timona L\Documents\hotel_data.csv"

# Save the DataFrame as a CSV file to the specified path
df.to_csv(csv_path, index=False)

print(f"Hotel data loaded into a DataFrame and saved as {csv_path}.")


# In[5]:


import pandas as pd

# Define the CSV file path
csv_path = r"C:\Users\Timona L\Documents\hotel_data.csv"

# Load the data into a DataFrame
df = pd.read_csv(csv_path)

# Split the 'formatted_address' column into 'state' and 'city'
df[['state', 'city']] = df['formatted_address'].str.split(',', 1, expand=True)

# Save the transformed data as a new CSV file
new_csv_path = r"C:\Users\Timona L\Documents\transformed_hotel_data.csv"
df.to_csv(new_csv_path, index=False)

print(f"Transformed data saved as {new_csv_path}.")


# In[ ]:




