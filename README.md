# Google Play Store Apps Analysis

## Introduction
This project conducts a comprehensive analysis of the Android app market by comparing thousands of apps available in the Google Play Store.

## Dataset Overview
**Data Source:**
The dataset was originally scraped from the Google Play Store by Lavanya Gupta in 2018. You can find the original dataset [here](https://www.kaggle.com/lava18/google-play-store-apps).

## Setup and Dependencies

### Import Required Libraries
```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.cm as cm
import plotly.express as px
import numpy as np
```

### Configure Notebook Presentation
```python
pd.options.display.float_format = '{:,.2f}'.format
```

### Load the Dataset
```python
df_apps = pd.read_csv('apps.csv')
```

## Data Cleaning
### Inspect Dataset
```python
df_apps.shape
df_apps.describe()
df_apps.sample(5)
```

### Drop Unused Columns
```python
df_apps.drop(columns=['Last_Updated','Android_Version'], inplace=True)
```

### Handle Missing Values in Ratings
```python
df_apps_clean = df_apps.dropna(subset=['Rating'])
```

### Remove Duplicate Entries
```python
df_apps_clean.drop_duplicates(subset=['App', 'Type', 'Price'], keep='first', inplace=True)
```

## Data Analysis
### Highest Rated Apps
```python
df_apps_clean.sort_values('Rating',ascending=False)
```

### Largest Apps by Size (MBs)
```python
df_apps_clean[df_apps_clean['Size_MBs'] == df_apps_clean['Size_MBs'].max()].head(5)
```

### Most Installed Apps
```python
df_apps_clean = df_apps_clean.sort_values(by='Installs', ascending=False)
```

## Data Visualization
### Apps by Size Distribution
```python
plt.figure(figsize=(14, 8), dpi=120)
plt.plot(grouped_size_mbs_df.Size_MBs, grouped_size_mbs_df.Apps, linestyle='-', color='b', linewidth=2)
plt.xlabel("Megabytes")
plt.ylabel("Number of Apps")
plt.title("Total Applications by Size")
plt.show()
```

### Content Rating Distribution (Pie Chart)
```python
fig = px.pie(
    names=df_apps_clean.Content_Rating.value_counts().index,
    values=df_apps_clean.Content_Rating.value_counts().values,
    title='Content Rating and Reviews Chart',
    hole=0.5
)
fig.show()
```

### App Install Distribution (Box Plot)
```python
box = px.box(
    df_apps_clean,
    y='Installs',
    x='Type',
    color='Type',
    notched=True,
    points='all',
    title='How Many Downloads are Paid Apps Giving Up?'
)
box.update_layout(yaxis=dict(type='log'))
box.show()
```

## Revenue Analysis
### Estimated Revenue per Category
```python
median_paid_apps = df_paid_apps.groupby('Category').Revenue_Estimate.median().sort_values(ascending=False)
median_paid_apps = median_paid_apps.reset_index()
median_paid_apps.columns=['Category','Median_Revenue_Estimate']
```

### Revenue Distribution (Box Plot)
```python
fig = px.box(
    df_paid_apps,
    x="Category",
    y="Revenue_Estimate",
    title="How Much Can Paid Apps Earn?"
)
fig.update_layout(
    xaxis_title='Category',
    yaxis_title='Paid App Ballpark Revenue',
    yaxis=dict(type='log'),
    xaxis={'categoryorder':'min ascending'}
)
fig.show()
```

### Paid App Pricing Analysis
```python
fig = px.box(
    df_paid_apps,
    x="Category",
    y="Price",
    title="Price per Category?"
)
fig.update_layout(
    xaxis_title='Category',
    yaxis_title='Paid App Price',
    yaxis=dict(type='log'),
    xaxis={'categoryorder':'max descending'}
)
fig.show()
```

## Key Findings
- **Most apps are not profitable**, as their median revenue is below $30K.
- **Highly profitable categories**: Entertainment, Weather, Education, Food & Drink, Parenting, Health & Fitness, Auto & Vehicles, and Games.
- **Some categories have significant price outliers**, including:
  - Medical
  - Lifestyle
  - Family
  - Sports
  - Photography
  - Finance
  - Games
  - Tools
  - Personalization

## Conclusion
This analysis provides insights into the **Google Play Store market trends, pricing strategies, revenue generation, and user behavior**. Further research could involve **predictive modeling** to analyze market potential for upcoming apps.

---

### **Author**
Developed by Mei Zhang

## **Credits**
This project is based on the **100 Days of Code: Python Bootcamp** by [Angela Yu](https://www.udemy.com/course/100-days-of-code/).

