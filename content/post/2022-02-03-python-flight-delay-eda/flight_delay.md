---
title: NYC Flight Delay
author: Rawan ALQarni
date: '2022-01-27'
slug: flight_delay
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2022-02-03T13:35:11Z'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

## NYC Flights Delay Data in 2013 
##### Team 4
- Rawan  
- Manar 
- Marya
- Ali 

#### Flight delay dataset: This dataset contain 336776 row collected from 3 airports: EWR,LGA,JFK. We assume that this dataset was collected for evaluation purposes. 

#### Import libraries 


```python
import pandas as pd 
import numpy as np
import datetime as dt
import matplotlib.pyplot as plt
import seaborn as sns
```

#### Read the data 


```python
flight_data = pd.read_csv("flight_data.csv")
```

#### Explore the data


```python
flight_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 336776 entries, 0 to 336775
    Data columns (total 19 columns):
     #   Column          Non-Null Count   Dtype  
    ---  ------          --------------   -----  
     0   year            336776 non-null  int64  
     1   month           336776 non-null  int64  
     2   day             336776 non-null  int64  
     3   dep_time        328521 non-null  float64
     4   sched_dep_time  336776 non-null  int64  
     5   dep_delay       328521 non-null  float64
     6   arr_time        328063 non-null  float64
     7   sched_arr_time  336776 non-null  int64  
     8   arr_delay       327346 non-null  float64
     9   carrier         336776 non-null  object 
     10  flight          336776 non-null  int64  
     11  tailnum         334264 non-null  object 
     12  origin          336776 non-null  object 
     13  dest            336776 non-null  object 
     14  air_time        327346 non-null  float64
     15  distance        336776 non-null  int64  
     16  hour            336776 non-null  int64  
     17  minute          336776 non-null  int64  
     18  time_hour       336776 non-null  object 
    dtypes: float64(5), int64(9), object(5)
    memory usage: 48.8+ MB



```python
flight_data.shape
```




    (336776, 19)



##### Dictionary 
- dep_time >> the departure time in 0000 = 00:00   
- sched_dep_time >> the schedule the departure time for flight not the actual departure tieme in 0000 = 00:00  
- dep_delay  >> the duration of delay in mins 
- arr_time  >> the arrival time in 0000 = 00:00 
- sched_arr_time  >> the schedule the expected arrival time for flight not the actual arrival tieme in 0000 = 00:00
- arr_delay  >> the duration of delay in mins 
- carrier  >> airlines company there are diffrent carriers in dataset 
- flight  >> flight number 
- tailnum >> tail number is airplane number for the flight 
- origin >> departure airport of the flights 
- dest >> destinations 
- air_time >> the duration of the flight in mins 
- distance >> the distance from origin to destination in miles
- hour >> hour of schedule departure time 
- minute >> mins of schedule departure time
- time_hour >> date of the flight and the deprture hour only

- create new column for date 


```python
list1 = flight_data.loc[:,"year":'day']
flight_data['date'] = pd.to_datetime(list1)
```


```python
flight_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 336776 entries, 0 to 336775
    Data columns (total 20 columns):
     #   Column          Non-Null Count   Dtype         
    ---  ------          --------------   -----         
     0   year            336776 non-null  int64         
     1   month           336776 non-null  int64         
     2   day             336776 non-null  int64         
     3   dep_time        328521 non-null  float64       
     4   sched_dep_time  336776 non-null  int64         
     5   dep_delay       328521 non-null  float64       
     6   arr_time        328063 non-null  float64       
     7   sched_arr_time  336776 non-null  int64         
     8   arr_delay       327346 non-null  float64       
     9   carrier         336776 non-null  object        
     10  flight          336776 non-null  int64         
     11  tailnum         334264 non-null  object        
     12  origin          336776 non-null  object        
     13  dest            336776 non-null  object        
     14  air_time        327346 non-null  float64       
     15  distance        336776 non-null  int64         
     16  hour            336776 non-null  int64         
     17  minute          336776 non-null  int64         
     18  time_hour       336776 non-null  object        
     19  date            336776 non-null  datetime64[ns]
    dtypes: datetime64[ns](1), float64(5), int64(9), object(5)
    memory usage: 51.4+ MB


- lookup for duplicate 


```python
# No duplicated values 
flight_data.duplicated().sum()
```




    0



#### Handling missing value 

- lookup for null values 


```python
flight_data.isnull().sum()
```




    year                 0
    month                0
    day                  0
    dep_time          8255
    sched_dep_time       0
    dep_delay         8255
    arr_time          8713
    sched_arr_time       0
    arr_delay         9430
    carrier              0
    flight               0
    tailnum           2512
    origin               0
    dest                 0
    air_time          9430
    distance             0
    hour                 0
    minute               0
    time_hour            0
    date                 0
    dtype: int64




```python
check_null = flight_data[flight_data['dep_time'].isnull()]
check_null
 
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>838</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>1630</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1815</td>
      <td>NaN</td>
      <td>EV</td>
      <td>4308</td>
      <td>N18120</td>
      <td>EWR</td>
      <td>RDU</td>
      <td>NaN</td>
      <td>416</td>
      <td>16</td>
      <td>30</td>
      <td>1/1/2013 16:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>839</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>1935</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2240</td>
      <td>NaN</td>
      <td>AA</td>
      <td>791</td>
      <td>N3EHAA</td>
      <td>LGA</td>
      <td>DFW</td>
      <td>NaN</td>
      <td>1389</td>
      <td>19</td>
      <td>35</td>
      <td>1/1/2013 19:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>840</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>1500</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1825</td>
      <td>NaN</td>
      <td>AA</td>
      <td>1925</td>
      <td>N3EVAA</td>
      <td>LGA</td>
      <td>MIA</td>
      <td>NaN</td>
      <td>1096</td>
      <td>15</td>
      <td>0</td>
      <td>1/1/2013 15:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>841</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>600</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>901</td>
      <td>NaN</td>
      <td>B6</td>
      <td>125</td>
      <td>N618JB</td>
      <td>JFK</td>
      <td>FLL</td>
      <td>NaN</td>
      <td>1069</td>
      <td>6</td>
      <td>0</td>
      <td>1/1/2013 6:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>1777</th>
      <td>2013</td>
      <td>1</td>
      <td>2</td>
      <td>NaN</td>
      <td>1540</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1747</td>
      <td>NaN</td>
      <td>EV</td>
      <td>4352</td>
      <td>N10575</td>
      <td>EWR</td>
      <td>CVG</td>
      <td>NaN</td>
      <td>569</td>
      <td>15</td>
      <td>40</td>
      <td>2/1/2013 15:00</td>
      <td>2013-01-02</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
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
      <th>336771</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1455</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1634</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3393</td>
      <td>NaN</td>
      <td>JFK</td>
      <td>DCA</td>
      <td>NaN</td>
      <td>213</td>
      <td>14</td>
      <td>55</td>
      <td>30-09-2013 14:00</td>
      <td>2013-09-30</td>
    </tr>
    <tr>
      <th>336772</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>2200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2312</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3525</td>
      <td>NaN</td>
      <td>LGA</td>
      <td>SYR</td>
      <td>NaN</td>
      <td>198</td>
      <td>22</td>
      <td>0</td>
      <td>30-09-2013 22:00</td>
      <td>2013-09-30</td>
    </tr>
    <tr>
      <th>336773</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1210</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1330</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3461</td>
      <td>N535MQ</td>
      <td>LGA</td>
      <td>BNA</td>
      <td>NaN</td>
      <td>764</td>
      <td>12</td>
      <td>10</td>
      <td>30-09-2013 12:00</td>
      <td>2013-09-30</td>
    </tr>
    <tr>
      <th>336774</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>1159</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1344</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3572</td>
      <td>N511MQ</td>
      <td>LGA</td>
      <td>CLE</td>
      <td>NaN</td>
      <td>419</td>
      <td>11</td>
      <td>59</td>
      <td>30-09-2013 11:00</td>
      <td>2013-09-30</td>
    </tr>
    <tr>
      <th>336775</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>NaN</td>
      <td>840</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1020</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3531</td>
      <td>N839MQ</td>
      <td>LGA</td>
      <td>RDU</td>
      <td>NaN</td>
      <td>431</td>
      <td>8</td>
      <td>40</td>
      <td>30-09-2013 08:00</td>
      <td>2013-09-30</td>
    </tr>
  </tbody>
</table>
<p>8255 rows × 20 columns</p>
</div>




```python
#delete this subset >> 8255
flight_data.dropna(subset=['dep_time'], inplace=True)
flight_data.shape
```




    (328521, 20)




```python
flight_data.isnull().sum()
```




    year                 0
    month                0
    day                  0
    dep_time             0
    sched_dep_time       0
    dep_delay            0
    arr_time           458
    sched_arr_time       0
    arr_delay         1175
    carrier              0
    flight               0
    tailnum              0
    origin               0
    dest                 0
    air_time          1175
    distance             0
    hour                 0
    minute               0
    time_hour            0
    date                 0
    dtype: int64




```python
# we assume that is no arr_time, arr_delay for flights the flight may be was schedualed but get 
# canceled even after delay 
check_null3 = flight_data[flight_data['arr_time'].isnull()]
check_null3
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>754</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>2016.0</td>
      <td>1930</td>
      <td>46.0</td>
      <td>NaN</td>
      <td>2220</td>
      <td>NaN</td>
      <td>EV</td>
      <td>4204</td>
      <td>N14168</td>
      <td>EWR</td>
      <td>OKC</td>
      <td>NaN</td>
      <td>1325</td>
      <td>19</td>
      <td>30</td>
      <td>1/1/2013 19:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>1714</th>
      <td>2013</td>
      <td>1</td>
      <td>2</td>
      <td>2041.0</td>
      <td>2045</td>
      <td>-4.0</td>
      <td>NaN</td>
      <td>2359</td>
      <td>NaN</td>
      <td>B6</td>
      <td>147</td>
      <td>N630JB</td>
      <td>JFK</td>
      <td>RSW</td>
      <td>NaN</td>
      <td>1074</td>
      <td>20</td>
      <td>45</td>
      <td>2/1/2013 20:00</td>
      <td>2013-01-02</td>
    </tr>
    <tr>
      <th>1756</th>
      <td>2013</td>
      <td>1</td>
      <td>2</td>
      <td>2145.0</td>
      <td>2129</td>
      <td>16.0</td>
      <td>NaN</td>
      <td>33</td>
      <td>NaN</td>
      <td>UA</td>
      <td>1299</td>
      <td>N12221</td>
      <td>EWR</td>
      <td>RSW</td>
      <td>NaN</td>
      <td>1068</td>
      <td>21</td>
      <td>29</td>
      <td>2/1/2013 21:00</td>
      <td>2013-01-02</td>
    </tr>
    <tr>
      <th>7039</th>
      <td>2013</td>
      <td>1</td>
      <td>9</td>
      <td>615.0</td>
      <td>615</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>855</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3856</td>
      <td>N161PQ</td>
      <td>JFK</td>
      <td>ATL</td>
      <td>NaN</td>
      <td>760</td>
      <td>6</td>
      <td>15</td>
      <td>9/1/2013 6:00</td>
      <td>2013-01-09</td>
    </tr>
    <tr>
      <th>7851</th>
      <td>2013</td>
      <td>1</td>
      <td>9</td>
      <td>2042.0</td>
      <td>2040</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>2357</td>
      <td>NaN</td>
      <td>B6</td>
      <td>677</td>
      <td>N807JB</td>
      <td>JFK</td>
      <td>LAX</td>
      <td>NaN</td>
      <td>2475</td>
      <td>20</td>
      <td>40</td>
      <td>9/1/2013 20:00</td>
      <td>2013-01-09</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
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
      <th>328710</th>
      <td>2013</td>
      <td>9</td>
      <td>22</td>
      <td>1244.0</td>
      <td>1215</td>
      <td>29.0</td>
      <td>NaN</td>
      <td>1405</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3616</td>
      <td>N501MQ</td>
      <td>LGA</td>
      <td>MSP</td>
      <td>NaN</td>
      <td>1020</td>
      <td>12</td>
      <td>15</td>
      <td>22-09-2013 12:00</td>
      <td>2013-09-22</td>
    </tr>
    <tr>
      <th>330305</th>
      <td>2013</td>
      <td>9</td>
      <td>24</td>
      <td>623.0</td>
      <td>625</td>
      <td>-2.0</td>
      <td>NaN</td>
      <td>800</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>3525</td>
      <td>N735MQ</td>
      <td>LGA</td>
      <td>RDU</td>
      <td>NaN</td>
      <td>431</td>
      <td>6</td>
      <td>25</td>
      <td>24-09-2013 06:00</td>
      <td>2013-09-24</td>
    </tr>
    <tr>
      <th>330418</th>
      <td>2013</td>
      <td>9</td>
      <td>24</td>
      <td>800.0</td>
      <td>800</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>933</td>
      <td>NaN</td>
      <td>UA</td>
      <td>544</td>
      <td>N827UA</td>
      <td>LGA</td>
      <td>ORD</td>
      <td>NaN</td>
      <td>733</td>
      <td>8</td>
      <td>0</td>
      <td>24-09-2013 08:00</td>
      <td>2013-09-24</td>
    </tr>
    <tr>
      <th>334177</th>
      <td>2013</td>
      <td>9</td>
      <td>27</td>
      <td>2253.0</td>
      <td>1945</td>
      <td>188.0</td>
      <td>NaN</td>
      <td>2146</td>
      <td>NaN</td>
      <td>EV</td>
      <td>5306</td>
      <td>N605QX</td>
      <td>LGA</td>
      <td>GSO</td>
      <td>NaN</td>
      <td>461</td>
      <td>19</td>
      <td>45</td>
      <td>27-09-2013 19:00</td>
      <td>2013-09-27</td>
    </tr>
    <tr>
      <th>335805</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>559.0</td>
      <td>600</td>
      <td>-1.0</td>
      <td>NaN</td>
      <td>715</td>
      <td>NaN</td>
      <td>WN</td>
      <td>464</td>
      <td>N411WN</td>
      <td>EWR</td>
      <td>MDW</td>
      <td>NaN</td>
      <td>711</td>
      <td>6</td>
      <td>0</td>
      <td>30-09-2013 06:00</td>
      <td>2013-09-30</td>
    </tr>
  </tbody>
</table>
<p>458 rows × 20 columns</p>
</div>




```python
# delete 458 row from this subset 
flight_data.dropna(subset=['arr_time'], inplace=True)

```


```python
flight_data.isnull().sum()
```




    year                0
    month               0
    day                 0
    dep_time            0
    sched_dep_time      0
    dep_delay           0
    arr_time            0
    sched_arr_time      0
    arr_delay         717
    carrier             0
    flight              0
    tailnum             0
    origin              0
    dest                0
    air_time          717
    distance            0
    hour                0
    minute              0
    time_hour           0
    date                0
    dtype: int64




```python
# we can count this if we have time >> due to the lack of time 
check_null2 = flight_data[flight_data['arr_delay'].isnull()]
check_null2
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>flight</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>471</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1525.0</td>
      <td>1530</td>
      <td>-5.0</td>
      <td>1934.0</td>
      <td>1805</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>4525</td>
      <td>N719MQ</td>
      <td>LGA</td>
      <td>XNA</td>
      <td>NaN</td>
      <td>1147</td>
      <td>15</td>
      <td>30</td>
      <td>1/1/2013 15:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>477</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1528.0</td>
      <td>1459</td>
      <td>29.0</td>
      <td>2002.0</td>
      <td>1647</td>
      <td>NaN</td>
      <td>EV</td>
      <td>3806</td>
      <td>N17108</td>
      <td>EWR</td>
      <td>STL</td>
      <td>NaN</td>
      <td>872</td>
      <td>14</td>
      <td>59</td>
      <td>1/1/2013 14:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>615</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1740.0</td>
      <td>1745</td>
      <td>-5.0</td>
      <td>2158.0</td>
      <td>2020</td>
      <td>NaN</td>
      <td>MQ</td>
      <td>4413</td>
      <td>N739MQ</td>
      <td>LGA</td>
      <td>XNA</td>
      <td>NaN</td>
      <td>1147</td>
      <td>17</td>
      <td>45</td>
      <td>1/1/2013 17:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>643</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1807.0</td>
      <td>1738</td>
      <td>29.0</td>
      <td>2251.0</td>
      <td>2103</td>
      <td>NaN</td>
      <td>UA</td>
      <td>1228</td>
      <td>N31412</td>
      <td>EWR</td>
      <td>SAN</td>
      <td>NaN</td>
      <td>2425</td>
      <td>17</td>
      <td>38</td>
      <td>1/1/2013 17:00</td>
      <td>2013-01-01</td>
    </tr>
    <tr>
      <th>725</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>1939.0</td>
      <td>1840</td>
      <td>59.0</td>
      <td>29.0</td>
      <td>2151</td>
      <td>NaN</td>
      <td>9E</td>
      <td>3325</td>
      <td>N905XJ</td>
      <td>JFK</td>
      <td>DFW</td>
      <td>NaN</td>
      <td>1391</td>
      <td>18</td>
      <td>40</td>
      <td>1/1/2013 18:00</td>
      <td>2013-01-01</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
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
      <th>334197</th>
      <td>2013</td>
      <td>9</td>
      <td>28</td>
      <td>555.0</td>
      <td>600</td>
      <td>-5.0</td>
      <td>953.0</td>
      <td>753</td>
      <td>NaN</td>
      <td>EV</td>
      <td>5068</td>
      <td>N133EV</td>
      <td>EWR</td>
      <td>DTW</td>
      <td>NaN</td>
      <td>488</td>
      <td>6</td>
      <td>0</td>
      <td>28-09-2013 06:00</td>
      <td>2013-09-28</td>
    </tr>
    <tr>
      <th>334354</th>
      <td>2013</td>
      <td>9</td>
      <td>28</td>
      <td>847.0</td>
      <td>839</td>
      <td>8.0</td>
      <td>1130.0</td>
      <td>959</td>
      <td>NaN</td>
      <td>EV</td>
      <td>4510</td>
      <td>N14542</td>
      <td>EWR</td>
      <td>MKE</td>
      <td>NaN</td>
      <td>725</td>
      <td>8</td>
      <td>39</td>
      <td>28-09-2013 08:00</td>
      <td>2013-09-28</td>
    </tr>
    <tr>
      <th>334412</th>
      <td>2013</td>
      <td>9</td>
      <td>28</td>
      <td>1010.0</td>
      <td>1020</td>
      <td>-10.0</td>
      <td>1344.0</td>
      <td>1222</td>
      <td>NaN</td>
      <td>EV</td>
      <td>4412</td>
      <td>N12175</td>
      <td>EWR</td>
      <td>DSM</td>
      <td>NaN</td>
      <td>1017</td>
      <td>10</td>
      <td>20</td>
      <td>28-09-2013 10:00</td>
      <td>2013-09-28</td>
    </tr>
    <tr>
      <th>334495</th>
      <td>2013</td>
      <td>9</td>
      <td>28</td>
      <td>1214.0</td>
      <td>1225</td>
      <td>-11.0</td>
      <td>1801.0</td>
      <td>1510</td>
      <td>NaN</td>
      <td>AA</td>
      <td>300</td>
      <td>N488AA</td>
      <td>EWR</td>
      <td>DFW</td>
      <td>NaN</td>
      <td>1372</td>
      <td>12</td>
      <td>25</td>
      <td>28-09-2013 12:00</td>
      <td>2013-09-28</td>
    </tr>
    <tr>
      <th>335534</th>
      <td>2013</td>
      <td>9</td>
      <td>29</td>
      <td>1734.0</td>
      <td>1711</td>
      <td>23.0</td>
      <td>2159.0</td>
      <td>2020</td>
      <td>NaN</td>
      <td>UA</td>
      <td>327</td>
      <td>N463UA</td>
      <td>EWR</td>
      <td>PDX</td>
      <td>NaN</td>
      <td>2434</td>
      <td>17</td>
      <td>11</td>
      <td>29-09-2013 17:00</td>
      <td>2013-09-29</td>
    </tr>
  </tbody>
</table>
<p>717 rows × 20 columns</p>
</div>




```python
flight_data.dropna(subset=['arr_delay'], inplace=True)
```


```python
flight_data.dropna(subset=['air_time'], inplace=True)
```


```python
flight_data.isnull().sum()
```




    year              0
    month             0
    day               0
    dep_time          0
    sched_dep_time    0
    dep_delay         0
    arr_time          0
    sched_arr_time    0
    arr_delay         0
    carrier           0
    flight            0
    tailnum           0
    origin            0
    dest              0
    air_time          0
    distance          0
    hour              0
    minute            0
    time_hour         0
    date              0
    dtype: int64




```python
#flight_data['cal_time']= flight_data['arr_time'] - flight_data['sched_arr_time']
```


```python
flight_data.shape
```




    (327346, 20)




```python
sub = flight_data.loc[:,'dep_time':]
sub.describe()
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
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>flight</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
      <td>327346.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1348.789883</td>
      <td>1340.335098</td>
      <td>12.555156</td>
      <td>1501.908238</td>
      <td>1532.788426</td>
      <td>6.895377</td>
      <td>1943.104501</td>
      <td>150.686460</td>
      <td>1048.371314</td>
      <td>13.141010</td>
      <td>26.234116</td>
    </tr>
    <tr>
      <th>std</th>
      <td>488.319979</td>
      <td>467.413156</td>
      <td>40.065688</td>
      <td>532.888731</td>
      <td>497.979124</td>
      <td>44.633292</td>
      <td>1621.523684</td>
      <td>93.688305</td>
      <td>735.908523</td>
      <td>4.662063</td>
      <td>19.295918</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>500.000000</td>
      <td>-43.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>-86.000000</td>
      <td>1.000000</td>
      <td>20.000000</td>
      <td>80.000000</td>
      <td>5.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>907.000000</td>
      <td>905.000000</td>
      <td>-5.000000</td>
      <td>1104.000000</td>
      <td>1122.000000</td>
      <td>-17.000000</td>
      <td>544.000000</td>
      <td>82.000000</td>
      <td>509.000000</td>
      <td>9.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1400.000000</td>
      <td>1355.000000</td>
      <td>-2.000000</td>
      <td>1535.000000</td>
      <td>1554.000000</td>
      <td>-5.000000</td>
      <td>1467.000000</td>
      <td>129.000000</td>
      <td>888.000000</td>
      <td>13.000000</td>
      <td>29.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1744.000000</td>
      <td>1729.000000</td>
      <td>11.000000</td>
      <td>1940.000000</td>
      <td>1944.000000</td>
      <td>14.000000</td>
      <td>3412.000000</td>
      <td>192.000000</td>
      <td>1389.000000</td>
      <td>17.000000</td>
      <td>44.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2400.000000</td>
      <td>2359.000000</td>
      <td>1301.000000</td>
      <td>2400.000000</td>
      <td>2359.000000</td>
      <td>1272.000000</td>
      <td>8500.000000</td>
      <td>695.000000</td>
      <td>4983.000000</td>
      <td>23.000000</td>
      <td>59.000000</td>
    </tr>
  </tbody>
</table>
</div>



- Add now column to evaluate delayed flights 


```python
# we created a function to determine if the delay was resonable based on departure delay average == 13 
def determine(x):
    if x>13:
        x=True
    else:
        x=False
    return x
```


```python
# we created a now column 
flight_data['delay_evaluation'] = flight_data['dep_delay'].apply(determine)
```


```python
flight_data
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>dep_time</th>
      <th>sched_dep_time</th>
      <th>dep_delay</th>
      <th>arr_time</th>
      <th>sched_arr_time</th>
      <th>arr_delay</th>
      <th>carrier</th>
      <th>...</th>
      <th>tailnum</th>
      <th>origin</th>
      <th>dest</th>
      <th>air_time</th>
      <th>distance</th>
      <th>hour</th>
      <th>minute</th>
      <th>time_hour</th>
      <th>date</th>
      <th>delay_evaluation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>517.0</td>
      <td>515</td>
      <td>2.0</td>
      <td>830.0</td>
      <td>819</td>
      <td>11.0</td>
      <td>UA</td>
      <td>...</td>
      <td>N14228</td>
      <td>EWR</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1400</td>
      <td>5</td>
      <td>15</td>
      <td>1/1/2013 5:00</td>
      <td>2013-01-01</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>533.0</td>
      <td>529</td>
      <td>4.0</td>
      <td>850.0</td>
      <td>830</td>
      <td>20.0</td>
      <td>UA</td>
      <td>...</td>
      <td>N24211</td>
      <td>LGA</td>
      <td>IAH</td>
      <td>227.0</td>
      <td>1416</td>
      <td>5</td>
      <td>29</td>
      <td>1/1/2013 5:00</td>
      <td>2013-01-01</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>542.0</td>
      <td>540</td>
      <td>2.0</td>
      <td>923.0</td>
      <td>850</td>
      <td>33.0</td>
      <td>AA</td>
      <td>...</td>
      <td>N619AA</td>
      <td>JFK</td>
      <td>MIA</td>
      <td>160.0</td>
      <td>1089</td>
      <td>5</td>
      <td>40</td>
      <td>1/1/2013 5:00</td>
      <td>2013-01-01</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>544.0</td>
      <td>545</td>
      <td>-1.0</td>
      <td>1004.0</td>
      <td>1022</td>
      <td>-18.0</td>
      <td>B6</td>
      <td>...</td>
      <td>N804JB</td>
      <td>JFK</td>
      <td>BQN</td>
      <td>183.0</td>
      <td>1576</td>
      <td>5</td>
      <td>45</td>
      <td>1/1/2013 5:00</td>
      <td>2013-01-01</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>1</td>
      <td>1</td>
      <td>554.0</td>
      <td>600</td>
      <td>-6.0</td>
      <td>812.0</td>
      <td>837</td>
      <td>-25.0</td>
      <td>DL</td>
      <td>...</td>
      <td>N668DN</td>
      <td>LGA</td>
      <td>ATL</td>
      <td>116.0</td>
      <td>762</td>
      <td>6</td>
      <td>0</td>
      <td>1/1/2013 6:00</td>
      <td>2013-01-01</td>
      <td>False</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
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
      <th>336765</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>2240.0</td>
      <td>2245</td>
      <td>-5.0</td>
      <td>2334.0</td>
      <td>2351</td>
      <td>-17.0</td>
      <td>B6</td>
      <td>...</td>
      <td>N354JB</td>
      <td>JFK</td>
      <td>SYR</td>
      <td>41.0</td>
      <td>209</td>
      <td>22</td>
      <td>45</td>
      <td>30-09-2013 22:00</td>
      <td>2013-09-30</td>
      <td>False</td>
    </tr>
    <tr>
      <th>336766</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>2240.0</td>
      <td>2250</td>
      <td>-10.0</td>
      <td>2347.0</td>
      <td>7</td>
      <td>-20.0</td>
      <td>B6</td>
      <td>...</td>
      <td>N281JB</td>
      <td>JFK</td>
      <td>BUF</td>
      <td>52.0</td>
      <td>301</td>
      <td>22</td>
      <td>50</td>
      <td>30-09-2013 22:00</td>
      <td>2013-09-30</td>
      <td>False</td>
    </tr>
    <tr>
      <th>336767</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>2241.0</td>
      <td>2246</td>
      <td>-5.0</td>
      <td>2345.0</td>
      <td>1</td>
      <td>-16.0</td>
      <td>B6</td>
      <td>...</td>
      <td>N346JB</td>
      <td>JFK</td>
      <td>ROC</td>
      <td>47.0</td>
      <td>264</td>
      <td>22</td>
      <td>46</td>
      <td>30-09-2013 22:00</td>
      <td>2013-09-30</td>
      <td>False</td>
    </tr>
    <tr>
      <th>336768</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>2307.0</td>
      <td>2255</td>
      <td>12.0</td>
      <td>2359.0</td>
      <td>2358</td>
      <td>1.0</td>
      <td>B6</td>
      <td>...</td>
      <td>N565JB</td>
      <td>JFK</td>
      <td>BOS</td>
      <td>33.0</td>
      <td>187</td>
      <td>22</td>
      <td>55</td>
      <td>30-09-2013 22:00</td>
      <td>2013-09-30</td>
      <td>False</td>
    </tr>
    <tr>
      <th>336769</th>
      <td>2013</td>
      <td>9</td>
      <td>30</td>
      <td>2349.0</td>
      <td>2359</td>
      <td>-10.0</td>
      <td>325.0</td>
      <td>350</td>
      <td>-25.0</td>
      <td>B6</td>
      <td>...</td>
      <td>N516JB</td>
      <td>JFK</td>
      <td>PSE</td>
      <td>196.0</td>
      <td>1617</td>
      <td>23</td>
      <td>59</td>
      <td>30-09-2013 23:00</td>
      <td>2013-09-30</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>327346 rows × 21 columns</p>
</div>



1- How many tail number we have in the dataset ? 



```python
flight_data['tailnum'].nunique()
```




    4037




```python
flight_data['tailnum'].value_counts() 
```




    N725MQ    544
    N722MQ    485
    N723MQ    475
    N711MQ    462
    N713MQ    449
             ... 
    N456UW      1
    N26906      1
    N809NW      1
    N941UW      1
    N557AS      1
    Name: tailnum, Length: 4037, dtype: int64




```python
# count the delyed flight for each tailnum and show total flight 
flight_data.groupby(['tailnum']).agg({'delay_evaluation':'sum', 'tailnum':'count'}).sort_values(by='delay_evaluation', ascending=False) 
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
      <th>delay_evaluation</th>
      <th>tailnum</th>
    </tr>
    <tr>
      <th>tailnum</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>N15910</th>
      <td>118</td>
      <td>265</td>
    </tr>
    <tr>
      <th>N258JB</th>
      <td>117</td>
      <td>420</td>
    </tr>
    <tr>
      <th>N725MQ</th>
      <td>116</td>
      <td>544</td>
    </tr>
    <tr>
      <th>N228JB</th>
      <td>114</td>
      <td>380</td>
    </tr>
    <tr>
      <th>N12921</th>
      <td>106</td>
      <td>265</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>N675MC</th>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>N68159</th>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>N302AS</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>N306AS</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>N565AS</th>
      <td>0</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>4037 rows × 2 columns</p>
</div>




```python
# we assign the subset to a dataframe 
tailnum_data = flight_data.groupby(['tailnum']).agg({'delay_evaluation':'sum', 'tailnum':'count'}).sort_values(by='delay_evaluation', ascending=False) 
```


```python
# we added now column to calculate the probability of delay for each tailnum 
tailnum_data['delay_probability'] = (tailnum_data['delay_evaluation'] / tailnum_data['tailnum'])*100  
tailnum_data
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
      <th>delay_evaluation</th>
      <th>tailnum</th>
      <th>delay_probability</th>
    </tr>
    <tr>
      <th>tailnum</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>N15910</th>
      <td>118</td>
      <td>265</td>
      <td>44.528302</td>
    </tr>
    <tr>
      <th>N258JB</th>
      <td>117</td>
      <td>420</td>
      <td>27.857143</td>
    </tr>
    <tr>
      <th>N725MQ</th>
      <td>116</td>
      <td>544</td>
      <td>21.323529</td>
    </tr>
    <tr>
      <th>N228JB</th>
      <td>114</td>
      <td>380</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>N12921</th>
      <td>106</td>
      <td>265</td>
      <td>40.000000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>N675MC</th>
      <td>0</td>
      <td>5</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>N68159</th>
      <td>0</td>
      <td>5</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>N302AS</th>
      <td>0</td>
      <td>1</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>N306AS</th>
      <td>0</td>
      <td>1</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>N565AS</th>
      <td>0</td>
      <td>3</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>4037 rows × 3 columns</p>
</div>



2- How many flights per carrier are there?  



```python
flight_data['carrier'].value_counts()
```




    UA    57782
    B6    54049
    EV    51108
    DL    47658
    AA    31947
    MQ    25037
    US    19831
    9E    17294
    WN    12044
    VX     5116
    FL     3175
    AS      709
    F9      681
    YV      544
    HA      342
    OO       29
    Name: carrier, dtype: int64



3- How many carrier we have in the dataset? 



```python
flight_data['carrier'].nunique()
```




    16



4- How many origin in the dataset?  


```python
flight_data['origin'].unique()
```




    array(['EWR', 'LGA', 'JFK'], dtype=object)



- Explore destination and distance  columns

5- How many destinations are there in the dataset? 


```python
flight_data['dest'].unique()
```




    array(['IAH', 'MIA', 'BQN', 'ATL', 'ORD', 'FLL', 'IAD', 'MCO', 'PBI',
           'TPA', 'LAX', 'SFO', 'DFW', 'BOS', 'LAS', 'MSP', 'DTW', 'RSW',
           'SJU', 'PHX', 'BWI', 'CLT', 'BUF', 'DEN', 'SNA', 'MSY', 'SLC',
           'XNA', 'MKE', 'SEA', 'ROC', 'SYR', 'SRQ', 'RDU', 'CMH', 'JAX',
           'CHS', 'MEM', 'PIT', 'SAN', 'DCA', 'CLE', 'STL', 'MYR', 'JAC',
           'MDW', 'HNL', 'BNA', 'AUS', 'BTV', 'PHL', 'STT', 'EGE', 'AVL',
           'PWM', 'IND', 'SAV', 'CAK', 'HOU', 'LGB', 'DAY', 'ALB', 'BDL',
           'MHT', 'MSN', 'GSO', 'CVG', 'BUR', 'RIC', 'GSP', 'GRR', 'MCI',
           'ORF', 'SAT', 'SDF', 'PDX', 'SJC', 'OMA', 'CRW', 'OAK', 'SMF',
           'TYS', 'PVD', 'DSM', 'PSE', 'TUL', 'BHM', 'OKC', 'CAE', 'HDN',
           'BZN', 'MTJ', 'EYW', 'PSP', 'ACK', 'BGR', 'ABQ', 'ILM', 'MVY',
           'SBN', 'LEX', 'CHO', 'TVC', 'ANC'], dtype=object)



6- How many flight to each destination per month? 


```python
# we suggested that some destination have higher attention in some season  
flight_data.groupby(['dest','month']).month.count()
```




    dest  month
    ABQ   4         9
          5        31
          6        30
          7        31
          8        31
                   ..
    XNA   8        87
          9        80
          10       91
          11       72
          12       62
    Name: month, Length: 1112, dtype: int64



7- What is the longest distance from each origin to other destination?


```python
flight_data.distance.max()
```




    4983




```python
flight_data.groupby(['origin','dest']).agg({'distance':'max'}).sort_values(by='distance', ascending=False) 

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
      <th></th>
      <th>distance</th>
    </tr>
    <tr>
      <th>origin</th>
      <th>dest</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>JFK</th>
      <th>HNL</th>
      <td>4983</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">EWR</th>
      <th>HNL</th>
      <td>4963</td>
    </tr>
    <tr>
      <th>ANC</th>
      <td>3370</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">JFK</th>
      <th>SFO</th>
      <td>2586</td>
    </tr>
    <tr>
      <th>OAK</th>
      <td>2576</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">EWR</th>
      <th>ALB</th>
      <td>143</td>
    </tr>
    <tr>
      <th>BDL</th>
      <td>116</td>
    </tr>
    <tr>
      <th>LGA</th>
      <th>PHL</th>
      <td>96</td>
    </tr>
    <tr>
      <th>JFK</th>
      <th>PHL</th>
      <td>94</td>
    </tr>
    <tr>
      <th>EWR</th>
      <th>PHL</th>
      <td>80</td>
    </tr>
  </tbody>
</table>
<p>223 rows × 1 columns</p>
</div>



- Explore arrival and departure delay columns

8- Sort the carriers based on their maximum departure delay?


```python
flight_data.groupby(['carrier']).agg({'dep_delay':'max'}).sort_values(by='dep_delay', ascending=False)
# HA 
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
      <th>dep_delay</th>
    </tr>
    <tr>
      <th>carrier</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>HA</th>
      <td>1301.0</td>
    </tr>
    <tr>
      <th>MQ</th>
      <td>1137.0</td>
    </tr>
    <tr>
      <th>AA</th>
      <td>1014.0</td>
    </tr>
    <tr>
      <th>DL</th>
      <td>960.0</td>
    </tr>
    <tr>
      <th>F9</th>
      <td>853.0</td>
    </tr>
    <tr>
      <th>9E</th>
      <td>747.0</td>
    </tr>
    <tr>
      <th>VX</th>
      <td>653.0</td>
    </tr>
    <tr>
      <th>FL</th>
      <td>602.0</td>
    </tr>
    <tr>
      <th>EV</th>
      <td>548.0</td>
    </tr>
    <tr>
      <th>B6</th>
      <td>502.0</td>
    </tr>
    <tr>
      <th>US</th>
      <td>500.0</td>
    </tr>
    <tr>
      <th>UA</th>
      <td>483.0</td>
    </tr>
    <tr>
      <th>WN</th>
      <td>471.0</td>
    </tr>
    <tr>
      <th>YV</th>
      <td>387.0</td>
    </tr>
    <tr>
      <th>AS</th>
      <td>225.0</td>
    </tr>
    <tr>
      <th>OO</th>
      <td>154.0</td>
    </tr>
  </tbody>
</table>
</div>



9- Which months have the most arrival delay? 



```python
flight_data.groupby(['month']).agg({'arr_delay':'mean'}).sort_values(by='arr_delay', ascending=False)
# 7,6,12 >> holiday months 
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
      <th>arr_delay</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>16.711307</td>
    </tr>
    <tr>
      <th>6</th>
      <td>16.481330</td>
    </tr>
    <tr>
      <th>12</th>
      <td>14.870355</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11.176063</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.129972</td>
    </tr>
    <tr>
      <th>8</th>
      <td>6.040652</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5.807577</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.613019</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3.521509</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.461347</td>
    </tr>
    <tr>
      <th>10</th>
      <td>-0.167063</td>
    </tr>
    <tr>
      <th>9</th>
      <td>-4.018364</td>
    </tr>
  </tbody>
</table>
</div>



11- Which months have the most depature delay? 


```python
# mean departed delay according to each month 
flight_data.groupby(['month']).agg({'dep_delay':'mean'}).sort_values(by='dep_delay', ascending=False)

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
      <th>dep_delay</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>21.522179</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20.725614</td>
    </tr>
    <tr>
      <th>12</th>
      <td>16.482161</td>
    </tr>
    <tr>
      <th>4</th>
      <td>13.849187</td>
    </tr>
    <tr>
      <th>3</th>
      <td>13.164289</td>
    </tr>
    <tr>
      <th>5</th>
      <td>12.891709</td>
    </tr>
    <tr>
      <th>8</th>
      <td>12.570524</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10.760239</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.985491</td>
    </tr>
    <tr>
      <th>9</th>
      <td>6.630285</td>
    </tr>
    <tr>
      <th>10</th>
      <td>6.233175</td>
    </tr>
    <tr>
      <th>11</th>
      <td>5.420340</td>
    </tr>
  </tbody>
</table>
</div>



12- How many flights depart and arrive on schedule? 


```python
flight_data['no_delay'] = (flight_data.dep_delay == 0) & (flight_data.arr_delay == 0)
flight_data.groupby(['origin']).agg({'no_delay':'sum'}).sort_values(by='no_delay', ascending=False)
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
      <th>no_delay</th>
    </tr>
    <tr>
      <th>origin</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>JFK</th>
      <td>131</td>
    </tr>
    <tr>
      <th>EWR</th>
      <td>117</td>
    </tr>
    <tr>
      <th>LGA</th>
      <td>99</td>
    </tr>
  </tbody>
</table>
</div>



### Data Visulasation 

1- display departure delay for each origin per month 


```python
sns.set(font_scale=1.5)
plt.style.use('ggplot')

from matplotlib import rcParams
# figure size in inches
rcParams['figure.figsize'] = 8, 4
sns.set(rc = {'figure.figsize':(15,8)})
```


```python
sns.set_theme(style="white")
```


```python
sns.barplot('month','dep_delay','origin', data=flight_data ,palette="Greens_d")
plt.title("Monthly Delayed Flight for each Origin")
plt.xlabel("Month")
plt.ylabel("Departure Delay (min)");
print("This plot display the monthly delayed flights at each airport")

```

    /home/rawan/.local/lib/python3.8/site-packages/seaborn/_decorators.py:36: FutureWarning: Pass the following variables as keyword args: x, y, hue. From version 0.12, the only valid positional argument will be `data`, and passing other arguments without an explicit keyword will result in an error or misinterpretation.
      warnings.warn(


    This plot display the monthly delayed flights at each airport



    
![png](output_68_2.png)
    



```python
#sns.set_theme(style="whitegrid")

#g = sns.catplot(
#    data=flight_data, kind="bar",
#    x="month", y="dep_delay", hue="origin",
# palette="dark", height=10
#)
#g.despine(left=True)
#g.set_titles("Monthly Departure Delayed Flight")
#g.set_axis_labels("Month", "Departure Delay (min)")
```

2- Display delay probability for each tail number (airplane)


```python
sns.scatterplot(y='delay_probability' , x=tailnum_data.index ,data =tailnum_data);
```


    
![png](output_71_0.png)
    


3- Display number of flights for each carrier in 2013 


```python
sns.histplot(flight_data['carrier'])
```




    <AxesSubplot:xlabel='carrier', ylabel='Count'>




    
![png](output_73_1.png)
    


#### conclusion 
- There were two factors that affect the flight schedule: 
   - seasons -  time of year could affect the flights, where there was a riase in delayed flights
   - tail number - diffrent airplane have different chances to be delayed.

- Outcomes:
   - Airlines carries need to prepare more on holiday seasons such as summer (6 june ,7 july) 
   - Airlines carries evaluate airplanes, because some airplanes need more maintaince to optimize the performance.   

