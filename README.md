# Airline Dataset Exploratory Data Analysis (EDA)

This project involves the Exploratory Data Analysis (EDA) of an airline dataset. The dataset contains various details about passengers, flights, airports, and flight statuses. Below is a detailed walkthrough of the analysis performed.

## Table of Contents
1. [Importing Libraries](#importing-libraries)
2. [Loading the Dataset](#loading-the-dataset)
3. [Initial Data Exploration](#initial-data-exploration)
4. [Data Cleaning](#data-cleaning)
5. [Exploratory Data Analysis](#exploratory-data-analysis)
    - [Gender Distribution](#gender-distribution)
    - [Age Distribution](#age-distribution)
    - [Nationality Distribution](#nationality-distribution)
    - [Airport Analysis](#airport-analysis)

## Importing Libraries

```python
import numpy as np 
import pandas as pd 
import matplotlib.pyplot as plt
import seaborn as sns 
import missingno as msno
import plotly.express as px
import folium 
from folium.plugins import HeatMap
import warnings
warnings.filterwarnings('ignore')


## Loading the Dataset

```python
import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))
        
df = pd.read_csv('/kaggle/input/airline-dataset/Airline Dataset Updated - v2.csv')
df.head(10)
```

## Initial Data Exploration

- The dataset contains 98,619 entries and 15 columns.
- Checking the datatypes and null values:

```python
df.info()
df.isnull().sum()
```

## Data Cleaning

- Dropping irrelevant columns: `First Name`, `Last Name`, `Passenger ID`

```python
df = df.drop(['First Name', 'Last Name', 'Passenger ID'], axis=1)
df.head(5)
```

## Exploratory Data Analysis

### Gender Distribution

- Unique values in `Gender` column:

```python
df['Gender'].unique()
```

- Count of each gender:

```python
data = df['Gender'].value_counts().reset_index()
```

- Visualization:

```python
fig = px.bar(data, x='index', y='Gender', color='index', color_discrete_sequence=px.colors.sequential.Agsunset, template='plotly_dark')
fig.update_layout(title_text='Number of Males & Females', xaxis_title='GENDER', yaxis_title='COUNT')
fig.show()
```

### Age Distribution

- KDE plot for age distribution:

```python
from seaborn import kdeplot
kdeplot(data=df, x='Age', hue='Gender')
```

### Nationality Distribution

- Unique nationalities and their count:

```python
df['Nationality'].unique()
df['Nationality'].nunique()
```

- Top 10 nationalities:

```python
nation_count = df['Nationality'].value_counts().reset_index()
top_10_countries = nation_count.nlargest(10, 'Nationality')
px.bar(top_10_countries, x='index', y='Nationality', color='index', color_discrete_sequence=px.colors.sequential.Agsunset, template='plotly_dark')
```

- Lowest 10 nationalities:

```python
lowest_10_countries = nation_count.nsmallest(10, 'Nationality')
px.bar(lowest_10_countries, x='index', y='Nationality', color='index', color_discrete_sequence=px.colors.sequential.Agsunset, template='plotly_dark')
```

### Airport Analysis

- Unique airports and their count:

```python
df['Airport Name'].unique()
df['Airport Name'].nunique()
```

- Top 10 airports with the highest number of passengers:

```python
airport_name = df['Airport Name'].value_counts().reset_index()
top10_airport = airport_name.nlargest(10, 'Airport Name')
px.bar(top10_airport, x='Airport Name', y='count', color='Airport Name', color_discrete_sequence=px.colors.sequential.Agsunset, template='plotly_dark')
```

- Top 10 airports with the lowest number of passengers:

```python
bottom10_airport = airport_name.nsmallest(10, 'Airport Name')
px.bar(bottom10_airport, x='Airport Name', y='count', color='Airport Name', color_discrete_sequence=px.colors.sequential.Agsunset, template='plotly_dark')
```

## Conclusion

This exploratory data analysis provides insights into the demographics and travel patterns of airline passengers. The visualizations help in understanding the distribution of genders, ages, nationalities, and the most and least frequented airports.
