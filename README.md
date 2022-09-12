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
