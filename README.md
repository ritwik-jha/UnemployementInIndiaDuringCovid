# Unemployement in India during covid data analysis
---

## Introduction
We are going to perform data analysis of covid unemployment data using python and will extract some insights from the data. 

## Data
The data used in this project can be found [here](https://www.kaggle.com/datasets/gokulrajkmv/unemployment-in-india)

## Modules
We are going to use the following python modules for this project:
1. Pandas
2. Numpy
3. Seaborn
4. Plotly
5. Geopandas
6. Shapely

---

## Step-1 (Importing the modules)
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as ex
import geopandas as gpd
from shapely.geometry import Point
```

## Step-2 (Loading the data)
```
data = pd.read_csv("data_path")
data.head()
```

![image](https://user-images.githubusercontent.com/59885389/189671504-ecd93e7b-6071-4c2e-b5e6-2501410dab67.png)

## Step-3 (Data Preprocessing)
1. We start by putting proper headers
```
data.columns = ["State", "Date", "Frequency", "Unemployment Rate", "Estimated Employed",
                "Estimated Labour Participation Rate", "Region", "Longitude", "Latitude"]
```
2. We add a month column to the data frame whose value will depend on the Date value
```
month = [data.iloc[i]["Date"].split('-')[1] for i in range(267)]
data['Month'] = [getMonth(num) for num in month]
```
3. Now we create 3 dataframes containing the data grouped by month, state and region
```
dataGroupMonth = data.groupby(["Month"], sort = False).mean()
```
![image](https://user-images.githubusercontent.com/59885389/189672776-c39da250-409d-4b90-8ff6-0219551fd26c.png)

```
dataGroupState = data.groupby(["State"]).mean()
```
![image](https://user-images.githubusercontent.com/59885389/189672977-a63cae90-46bf-4132-9689-800dd2d30b14.png)

```
dataGroupRegion = data.groupby(["Region"]).mean()
```
![image](https://user-images.githubusercontent.com/59885389/189673190-9946a036-3dca-45ff-9114-c2abfdbbaa12.png)

## Step-4 (Plotting Graphs)
1. Plotting Bar Graph for Unemployement Rate vs Month
```
fig = plt.figure(figsize = (10, 5))
months = ["January", "Feburary", "March", "April", "May","June", "July", "August", "September", "October"]
 
# creating the bar plot
plt.bar(months, dataGroupMonth["Unemployment Rate"], color ='maroon',
        width = 0.4)
 
plt.xlabel("Months")
plt.ylabel("Unemployment Rate")
plt.title("Unemployement Rate in India in 2020")
plt.show()
```
![image](https://user-images.githubusercontent.com/59885389/189673696-886bdc15-25a7-4925-a63d-368aabaca5fd.png)

### From the graph we can conclude that the unemployement rate peaked during the lockdown period but then decreased gradually as the lockdown was lifted. 

2. Plotting Sunburst graph for Unemployment Rate vs Region
```
fig = plt.figure(figsize = (10, 5))
regions = ["East", "North", "Northeast", "South", "West"]
 
unempRate = dataGroupRegion['Unemployment Rate']

unemploment = {"Regions": regions, "Unemp Rate": unempRate}
unemploment = pd.DataFrame(unemploment)
figure = ex.sunburst(unemploment, path=["Regions"], 
                     values="Unemp Rate", 
                     width=700, height=700, color_continuous_scale="RdY1Gn", 
                     title="Unemployment Rate in India")
figure.show()
```
![image](https://user-images.githubusercontent.com/59885389/189674391-91638ce0-877b-4162-99f6-2d20052c7399.png)

### From the char we can conclude that the the northern and eastern region had the highest average unemployemt rate in 2020

3. Plotting the geoplot for Unemployement Rate vs State
```
#creating the map dataframe using which we plot the map of india
fp = r'./india-polygon.shp'
map_df = gpd.read_file(fp) 
map_df_copy = gpd.read_file(fp)
map_df.head()
```
![image](https://user-images.githubusercontent.com/59885389/189674865-19eb11c2-1d7e-4592-b024-221b0e19f34a.png)
```
map_df.plot()
```
![image](https://user-images.githubusercontent.com/59885389/189674960-982c019f-125f-48d6-8784-6d8b0c92b03e.png)
```
#creating the states dataframe that stores the uenmployment rate per state
states = data["state"].unique()
unempRatePerState = dataGroupState['Unemployment Rate']
unempRatePerStateData = {"State": states, "Unemp Rate": unempRatePerState}
stateUnemp_df = pd.DataFrame(unempRatePerStateData)
```

```
#Merging the data
merged = map_df.set_index('st_nm').join(stateUnemp_df.set_index('State'))
merged['Unemp Rate'] = merged['Unemp Rate'].replace(np.nan, 0)
merged.head()
```

```
#Create figure and axes for Matplotlib and set the title
fig, ax = plt.subplots(1, figsize=(10, 10))
ax.axis('off')
ax.set_title('Unemployment Rate in India during 2020 state-wise', fontdict={'fontsize': '20', 'fontweight' : '10'})
# Plot the figure
merged.plot(column='Unemp Rate',cmap='YlOrRd', linewidth=0.8, ax=ax, edgecolor='0',legend=True,markersize=[39.739192, -104.990337], 
            legend_kwds={'label': "Unemployment Rate"})
```

![image](https://user-images.githubusercontent.com/59885389/189675688-bdbaeee7-56d2-4b7f-a8d9-468571e80757.png)
### The map shows the the state wise average unemployment rate in the year 2020
