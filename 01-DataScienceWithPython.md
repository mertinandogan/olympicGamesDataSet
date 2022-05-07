```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from collections import Counter

# Python'da uyarıları kapatalım.
import warnings 
warnings.filterwarnings("ignore")
```


```python
veri = pd.read_csv("olimpiyatlar.csv")
veri.head()
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
      <th>ID</th>
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Games</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992 Summer</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>A Lamusi</td>
      <td>M</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012 Summer</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Gunnar Nielsen Aaby</td>
      <td>M</td>
      <td>24.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark</td>
      <td>DEN</td>
      <td>1920 Summer</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Football</td>
      <td>Football Men's Football</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900 Summer</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Christine Jacoba Aaftink</td>
      <td>F</td>
      <td>21.0</td>
      <td>185.0</td>
      <td>82.0</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1988 Winter</td>
      <td>1988</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Speed Skating</td>
      <td>Speed Skating Women's 500 metres</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
veri.info
```




    <bound method DataFrame.info of             ID                      Name Gender   Age  Height  Weight  \
    0            1                 A Dijiang      M  24.0   180.0    80.0   
    1            2                  A Lamusi      M  23.0   170.0    60.0   
    2            3       Gunnar Nielsen Aaby      M  24.0     NaN     NaN   
    3            4      Edgar Lindenau Aabye      M  34.0     NaN     NaN   
    4            5  Christine Jacoba Aaftink      F  21.0   185.0    82.0   
    ...        ...                       ...    ...   ...     ...     ...   
    271111  135569                Andrzej ya      M  29.0   179.0    89.0   
    271112  135570                  Piotr ya      M  27.0   176.0    59.0   
    271113  135570                  Piotr ya      M  27.0   176.0    59.0   
    271114  135571        Tomasz Ireneusz ya      M  30.0   185.0    96.0   
    271115  135571        Tomasz Ireneusz ya      M  34.0   185.0    96.0   
    
                      Team  NOC        Games  Year  Season            City  \
    0                China  CHN  1992 Summer  1992  Summer       Barcelona   
    1                China  CHN  2012 Summer  2012  Summer          London   
    2              Denmark  DEN  1920 Summer  1920  Summer       Antwerpen   
    3       Denmark/Sweden  DEN  1900 Summer  1900  Summer           Paris   
    4          Netherlands  NED  1988 Winter  1988  Winter         Calgary   
    ...                ...  ...          ...   ...     ...             ...   
    271111        Poland-1  POL  1976 Winter  1976  Winter       Innsbruck   
    271112          Poland  POL  2014 Winter  2014  Winter           Sochi   
    271113          Poland  POL  2014 Winter  2014  Winter           Sochi   
    271114          Poland  POL  1998 Winter  1998  Winter          Nagano   
    271115          Poland  POL  2002 Winter  2002  Winter  Salt Lake City   
    
                    Sport                                     Event Medal  
    0          Basketball               Basketball Men's Basketball   NaN  
    1                Judo              Judo Men's Extra-Lightweight   NaN  
    2            Football                   Football Men's Football   NaN  
    3          Tug-Of-War               Tug-Of-War Men's Tug-Of-War  Gold  
    4       Speed Skating          Speed Skating Women's 500 metres   NaN  
    ...               ...                                       ...   ...  
    271111           Luge                Luge Mixed (Men)'s Doubles   NaN  
    271112    Ski Jumping  Ski Jumping Men's Large Hill, Individual   NaN  
    271113    Ski Jumping        Ski Jumping Men's Large Hill, Team   NaN  
    271114      Bobsleigh                      Bobsleigh Men's Four   NaN  
    271115      Bobsleigh                      Bobsleigh Men's Four   NaN  
    
    [271116 rows x 15 columns]>




```python
veri.columns
```




    Index(['ID', 'Name', 'Gender', 'Age', 'Height', 'Weight', 'Team', 'NOC',
           'Games', 'Year', 'Season', 'City', 'Sport', 'Event', 'Medal'],
          dtype='object')




```python
#Sütunların ismini değiştirmek için rename fonk kullanılır

# veri.rename
```

## Yararsız verinin çıkarılması ve düzenlenmesi


```python
# drop fonksiyonu id ve oyunlar sütunlarını çıkaralım
veri = veri.drop(["ID", "Games"], axis = 1) # axis = 1 sütun manasında
veri.head(2)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Dijiang</td>
      <td>M</td>
      <td>24.0</td>
      <td>180.0</td>
      <td>80.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A Lamusi</td>
      <td>M</td>
      <td>23.0</td>
      <td>170.0</td>
      <td>60.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Judo</td>
      <td>Judo Men's Extra-Lightweight</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Kayıp Veri Sorunu

## Boy ve Kilo Sütunu Kayıp Veri Doldurma
    "Boy ve kilo sütununda bulunan kayıp veriyi event ortalamasına göre dolduracağız."


```python
unique_event = pd.unique(veri.Event)
print(f"Number of unique event: {len(unique_event)}")
unique_event[:10]
```

    Number of unique event: 765





    array(["Basketball Men's Basketball", "Judo Men's Extra-Lightweight",
           "Football Men's Football", "Tug-Of-War Men's Tug-Of-War",
           "Speed Skating Women's 500 metres",
           "Speed Skating Women's 1,000 metres",
           "Cross Country Skiing Men's 10 kilometres",
           "Cross Country Skiing Men's 50 kilometres",
           "Cross Country Skiing Men's 10/15 kilometres Pursuit",
           "Cross Country Skiing Men's 4 x 10 kilometres Relay"], dtype=object)




```python
# her bir etkinliği iteratif olarak dolaş
# etkinlik özelinde boy ve kilo ortalamalarını hesapla
# etkinlik özelinde kayıp boy ve kilo değerlerini etkinlik ortalamalarına eşitle

temporary_data = veri.copy() # gerçek veriyi bozmamak için bir kopyasını oluşturalım 
weight_height_list = ["Weight","Height"]

for event in unique_event:   #liste içerisinde dolaş
    
    # etkinlik filtresi oluşturalım
    event_filter = temporary_data.Event == event
    # veriyi etkinliğe göre filtreleyelim
    filtered_data = temporary_data[event_filter]
    
    # boy ve kilo için etkinlik özelinde ortalamaları hesaplayalım
    for s in weight_height_list:
        average = np.round(np.mean(filtered_data[s]),2)
        if ~np.isnan(average): #eğer etkinlik özelinde ortalama varsa
            filtered_data[s] = filtered_data[s].fillna(average)
        else: #eğer etkinlik özelinde ortalama yoksa ortalamayı hesapla
            all_data_average = np.round(np.mean(veri[s]),2)
            filtered_data[s] = filtered_data[s].fillna(all_data_average)
    #etkinlik özelinde kayıp verileri doldurulmuş olan verileri temporary_data'ya eşitl
    temporary_data[event_filter] = filtered_data

# kayıp değerleri giderilmiş olan geçici veriyi gerçek veriye eşitle
veri = temporary_data.copy()
veri.info() # boy ve kilo sütunlarında kayıp değer sayısına bakalım
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 271116 entries, 0 to 271115
    Data columns (total 13 columns):
     #   Column  Non-Null Count   Dtype  
    ---  ------  --------------   -----  
     0   Name    271116 non-null  object 
     1   Gender  271116 non-null  object 
     2   Age     261642 non-null  float64
     3   Height  271116 non-null  float64
     4   Weight  271116 non-null  float64
     5   Team    271116 non-null  object 
     6   NOC     271116 non-null  object 
     7   Year    271116 non-null  int64  
     8   Season  271116 non-null  object 
     9   City    271116 non-null  object 
     10  Sport   271116 non-null  object 
     11  Event   271116 non-null  object 
     12  Medal   39783 non-null   object 
    dtypes: float64(3), int64(1), object(9)
    memory usage: 26.9+ MB


## Age Sütununda Kayıp Veri Doldurma
Yaş sütununda bulunan kayıp veriyi veri setinin yaş ortalamasına göre dolduracağız.


```python
# Age değişkeninde tanımlı olmayan değerleri bulalım

yas_ort = np.round(np.mean(veri.Age),2)
print(f"Yaş ortalaması : {yas_ort}")
veri["Age"] = veri["Age"].fillna(yas_ort)
veri.info()
```

    Yaş ortalaması : 25.56
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 271116 entries, 0 to 271115
    Data columns (total 13 columns):
     #   Column  Non-Null Count   Dtype  
    ---  ------  --------------   -----  
     0   Name    271116 non-null  object 
     1   Gender  271116 non-null  object 
     2   Age     271116 non-null  float64
     3   Height  271116 non-null  float64
     4   Weight  271116 non-null  float64
     5   Team    271116 non-null  object 
     6   NOC     271116 non-null  object 
     7   Year    271116 non-null  int64  
     8   Season  271116 non-null  object 
     9   City    271116 non-null  object 
     10  Sport   271116 non-null  object 
     11  Event   271116 non-null  object 
     12  Medal   39783 non-null   object 
    dtypes: float64(3), int64(1), object(9)
    memory usage: 26.9+ MB


## Madalya Alamayan Sporcuları Veri Setinden Çıkar


```python
medal_degiskeni = veri["Medal"]
pd.isnull(medal_degiskeni).sum()
```




    231333




```python
medal_degiskeni_filtresi = ~pd.isnull(medal_degiskeni)
```


```python
veri = veri[medal_degiskeni_filtresi]
veri.head(5)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>182.48</td>
      <td>95.62</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>182.01</td>
      <td>76.69</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 200 metres Breaststroke</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>177.00</td>
      <td>75.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 400 metres Breaststroke</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Juhamatti Tapio Aaltonen</td>
      <td>M</td>
      <td>28.0</td>
      <td>184.00</td>
      <td>85.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>2014</td>
      <td>Winter</td>
      <td>Sochi</td>
      <td>Ice Hockey</td>
      <td>Ice Hockey Men's Ice Hockey</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Paavo Johannes Aaltonen</td>
      <td>M</td>
      <td>28.0</td>
      <td>175.00</td>
      <td>64.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1948</td>
      <td>Summer</td>
      <td>London</td>
      <td>Gymnastics</td>
      <td>Gymnastics Men's Individual All-Around</td>
      <td>Bronze</td>
    </tr>
  </tbody>
</table>
</div>




```python
veri.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 39783 entries, 3 to 271103
    Data columns (total 13 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   Name    39783 non-null  object 
     1   Gender  39783 non-null  object 
     2   Age     39783 non-null  float64
     3   Height  39783 non-null  float64
     4   Weight  39783 non-null  float64
     5   Team    39783 non-null  object 
     6   NOC     39783 non-null  object 
     7   Year    39783 non-null  int64  
     8   Season  39783 non-null  object 
     9   City    39783 non-null  object 
     10  Sport   39783 non-null  object 
     11  Event   39783 non-null  object 
     12  Medal   39783 non-null  object 
    dtypes: float64(3), int64(1), object(9)
    memory usage: 4.2+ MB



```python
# Sonradan kullanabilmek için veriyi kaydedelim.
veri.to_csv("olimpiyatlar_temizlenmis.csv", index = False)
```

## Tek Değişkenli Veri Analizi


```python
# öncelikli olarak histogram grafiklerini çizdireceğimiz bir fonksiyon yazalım
def plotHistogram(degisken):
    """
    Girdi : Değişken/Sütun İsmi
    Çıktı : İlgili değişkenin histogramı
        
    """
    
    plt.figure()
    plt.hist(veri[degisken], bins = 85, color = "blue") # bins kaç aralığa böleceğimizi söyler
    plt.xlabel(degisken)
    plt.ylabel("Frequency")
    plt.title(f"Data Frequency - {degisken}")
    plt.show()
    
```


```python
# Tüm sayısal değerler için histogramları çizdirelim

sayisal_degisken = ["Age", "Height", "Weight", "Year"]
for i in sayisal_degisken:
    plotHistogram(i)
```


    
![png](output_21_0.png)
    



    
![png](output_21_1.png)
    



    
![png](output_21_2.png)
    



    
![png](output_21_3.png)
    



```python
veri.describe()
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>39783.000000</td>
      <td>39783.000000</td>
      <td>39783.000000</td>
      <td>39783.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>25.918456</td>
      <td>177.336690</td>
      <td>73.738320</td>
      <td>1973.943845</td>
    </tr>
    <tr>
      <th>std</th>
      <td>5.859569</td>
      <td>10.170124</td>
      <td>13.979041</td>
      <td>33.822857</td>
    </tr>
    <tr>
      <th>min</th>
      <td>10.000000</td>
      <td>136.000000</td>
      <td>28.000000</td>
      <td>1896.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>22.000000</td>
      <td>170.000000</td>
      <td>64.000000</td>
      <td>1952.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>25.000000</td>
      <td>177.480000</td>
      <td>73.000000</td>
      <td>1984.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>29.000000</td>
      <td>184.000000</td>
      <td>82.000000</td>
      <td>2002.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>73.000000</td>
      <td>223.000000</td>
      <td>182.000000</td>
      <td>2016.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Age değişkeni için kutu grafiği


```python
plt.boxplot(veri.Age)
plt.title("Box-plot graph for Age variable")
plt.xlabel("Age")
plt.ylabel("Value")
plt.show()
```


    
![png](output_24_0.png)
    


## Kategorik Değişkenler İçin Yapılabilecekler


```python
# öncelikle çubuk grafiğini çizdireceğimiz methodu yazalım
def plotBar(degisken, n =5):
    """
    Girdi : Değişken/Sütun İsmi
        n : Gösterilecek unique değer sayısı
    Çıktı : Çubuk grafiği
    """
    veri_ = veri[degisken]
    veri_sayma = veri_.value_counts()
    veri_sayma = veri_sayma[:n]
    plt.figure()
    plt.bar(veri_sayma.index, veri_sayma, color = "orange")
    plt.xticks(veri_sayma.index, veri_sayma.index.values)
    plt.xticks(rotation = 45)
    plt.ylabel("Frekans")
    plt.title(f"Veri Sıklığı - {degisken}")
    plt.show()
    print(f"{degisken} : \n {veri_sayma}")
    
```


```python
kategorik_degisken = ["Name", "Gender", "Team", "NOC", "Season","City","Event","Medal"]
for i in kategorik_degisken:
    plotBar(i)
```


    
![png](output_27_0.png)
    


    Name : 
     Michael Fred Phelps, II               28
    Larysa Semenivna Latynina (Diriy-)    18
    Nikolay Yefimovich Andrianov          15
    Ole Einar Bjrndalen                   13
    Edoardo Mangiarotti                   13
    Name: Name, dtype: int64



    
![png](output_27_2.png)
    


    Gender : 
     M    28530
    F    11253
    Name: Gender, dtype: int64



    
![png](output_27_4.png)
    


    Team : 
     United States    5219
    Soviet Union     2451
    Germany          1984
    Great Britain    1673
    France           1550
    Name: Team, dtype: int64



    
![png](output_27_6.png)
    


    NOC : 
     USA    5637
    URS    2503
    GER    2165
    GBR    2068
    FRA    1777
    Name: NOC, dtype: int64



    
![png](output_27_8.png)
    


    Season : 
     Summer    34088
    Winter     5695
    Name: Season, dtype: int64



    
![png](output_27_10.png)
    


    City : 
     London            3624
    Athina            2602
    Los Angeles       2123
    Beijing           2048
    Rio de Janeiro    2023
    Name: City, dtype: int64



    
![png](output_27_12.png)
    


    Event : 
     Football Men's Football        1269
    Ice Hockey Men's Ice Hockey    1230
    Hockey Men's Hockey            1050
    Water Polo Men's Water Polo     866
    Rowing Men's Coxed Eights       730
    Name: Event, dtype: int64



    
![png](output_27_14.png)
    


    Medal : 
     Gold      13372
    Bronze    13295
    Silver    13116
    Name: Medal, dtype: int64


## İki Değişkenli Veri Analizi

# Cinsiyete Göre Boy ve Ağırlık Karşılaştırması


```python
erkek = veri[veri.Gender == "M"]
erkek.head(3)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>182.48</td>
      <td>95.62</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>182.01</td>
      <td>76.69</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 200 metres Breaststroke</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>177.00</td>
      <td>75.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 400 metres Breaststroke</td>
      <td>Bronze</td>
    </tr>
  </tbody>
</table>
</div>




```python
kadin = veri[veri.Gender == "F"]
kadin.head(3)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>79</th>
      <td>Ragnhild Margrethe Aamodt</td>
      <td>F</td>
      <td>27.0</td>
      <td>163.00</td>
      <td>68.88</td>
      <td>Norway</td>
      <td>NOR</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Handball</td>
      <td>Handball Women's Handball</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Willemien Aardenburg</td>
      <td>F</td>
      <td>22.0</td>
      <td>166.13</td>
      <td>60.53</td>
      <td>Netherlands</td>
      <td>NED</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Hockey</td>
      <td>Hockey Women's Hockey</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Ann Kristin Aarnes</td>
      <td>F</td>
      <td>23.0</td>
      <td>182.00</td>
      <td>64.00</td>
      <td>Norway</td>
      <td>NOR</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Football</td>
      <td>Football Women's Football</td>
      <td>Bronze</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure()
plt.scatter(kadin.Height, kadin.Weight, alpha=0.4, label = "Kadın", color = "hotpink")
plt.scatter(erkek.Height, erkek.Weight, alpha=0.4, label = "Erkek", color = "blue")
plt.xlabel("Boy")
plt.ylabel("Kilo")
plt.title("Boy-Kilo Arasındaki İlişki")
plt.legend()
plt.show()
```


    
![png](output_32_0.png)
    


## Sayısal Sütunlar arasındaki ilişkinin incelenmesi


```python
veri.loc[:,["Age","Height","Weight"]].corr() #korelasyon tablosu oluşur
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Age</th>
      <td>1.000000</td>
      <td>0.061890</td>
      <td>0.136349</td>
    </tr>
    <tr>
      <th>Height</th>
      <td>0.061890</td>
      <td>1.000000</td>
      <td>0.794368</td>
    </tr>
    <tr>
      <th>Weight</th>
      <td>0.136349</td>
      <td>0.794368</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Madalya ve Yaş Arasındaki İlişki


```python
# Sporcuları gold,silver, bronze medal'a göre gruplandıralım
veri_gecici = veri.copy()
veri_gecici = pd.get_dummies(veri_gecici, columns=["Medal"])
veri_gecici.head(2)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>182.48</td>
      <td>95.62</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>182.01</td>
      <td>76.69</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 200 metres Breaststroke</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
veri_gecici.loc[:,["Age","Medal_Bronze","Medal_Gold","Medal_Silver"]].corr()
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
      <th>Age</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Age</th>
      <td>1.000000</td>
      <td>-0.005584</td>
      <td>-0.002576</td>
      <td>0.008192</td>
    </tr>
    <tr>
      <th>Medal_Bronze</th>
      <td>-0.005584</td>
      <td>1.000000</td>
      <td>-0.504110</td>
      <td>-0.496859</td>
    </tr>
    <tr>
      <th>Medal_Gold</th>
      <td>-0.002576</td>
      <td>-0.504110</td>
      <td>1.000000</td>
      <td>-0.499022</td>
    </tr>
    <tr>
      <th>Medal_Silver</th>
      <td>0.008192</td>
      <td>-0.496859</td>
      <td>-0.499022</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Takımların Kazandıkları Gold, Silver ve Bronze Medal Sayıları


```python
veri_gecici[["Team","Medal_Bronze","Medal_Gold","Medal_Silver"]].groupby(["Team"], as_index = False).sum().sort_values(by="Medal_Gold", ascending = False)[:10]
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
      <th>Team</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>462</th>
      <td>United States</td>
      <td>1233.0</td>
      <td>2474.0</td>
      <td>1512.0</td>
    </tr>
    <tr>
      <th>403</th>
      <td>Soviet Union</td>
      <td>677.0</td>
      <td>1058.0</td>
      <td>716.0</td>
    </tr>
    <tr>
      <th>165</th>
      <td>Germany</td>
      <td>678.0</td>
      <td>679.0</td>
      <td>627.0</td>
    </tr>
    <tr>
      <th>215</th>
      <td>Italy</td>
      <td>484.0</td>
      <td>535.0</td>
      <td>508.0</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Great Britain</td>
      <td>572.0</td>
      <td>519.0</td>
      <td>582.0</td>
    </tr>
    <tr>
      <th>149</th>
      <td>France</td>
      <td>577.0</td>
      <td>455.0</td>
      <td>518.0</td>
    </tr>
    <tr>
      <th>420</th>
      <td>Sweden</td>
      <td>507.0</td>
      <td>451.0</td>
      <td>476.0</td>
    </tr>
    <tr>
      <th>198</th>
      <td>Hungary</td>
      <td>365.0</td>
      <td>432.0</td>
      <td>330.0</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Canada</td>
      <td>408.0</td>
      <td>422.0</td>
      <td>413.0</td>
    </tr>
    <tr>
      <th>117</th>
      <td>East Germany</td>
      <td>263.0</td>
      <td>369.0</td>
      <td>309.0</td>
    </tr>
  </tbody>
</table>
</div>



## Kazanılan Madalyaların Hangi Şehirlerde Kazanıldığı


```python
 veri_gecici[["City","Medal_Bronze","Medal_Gold","Medal_Silver"]].groupby(["City"], as_index = False).sum().sort_values(by="Medal_Gold", ascending = False)[:10]
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
      <th>City</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>17</th>
      <td>London</td>
      <td>1214.0</td>
      <td>1215.0</td>
      <td>1195.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Athina</td>
      <td>860.0</td>
      <td>883.0</td>
      <td>859.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Los Angeles</td>
      <td>706.0</td>
      <td>726.0</td>
      <td>691.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Beijing</td>
      <td>710.0</td>
      <td>671.0</td>
      <td>667.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Rio de Janeiro</td>
      <td>703.0</td>
      <td>665.0</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Sydney</td>
      <td>680.0</td>
      <td>663.0</td>
      <td>661.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Atlanta</td>
      <td>629.0</td>
      <td>608.0</td>
      <td>605.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Barcelona</td>
      <td>604.0</td>
      <td>559.0</td>
      <td>549.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Seoul</td>
      <td>549.0</td>
      <td>520.0</td>
      <td>513.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Antwerpen</td>
      <td>367.0</td>
      <td>493.0</td>
      <td>448.0</td>
    </tr>
  </tbody>
</table>
</div>



## Çok Değişkenli Veri Analizi

## Pivot Tablosu


```python
veri_pivot = veri.pivot_table(index="Medal", columns= "Gender",
                             values = ["Height","Weight","Age"], 
                             aggfunc= {"Height":np.mean, "Weight":np.mean,"Age":[min,max,np.mean,np.std]})
veri_pivot.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="8" halign="left">Age</th>
      <th colspan="2" halign="left">Height</th>
      <th colspan="2" halign="left">Weight</th>
    </tr>
    <tr>
      <th></th>
      <th colspan="2" halign="left">max</th>
      <th colspan="2" halign="left">mean</th>
      <th colspan="2" halign="left">min</th>
      <th colspan="2" halign="left">std</th>
      <th colspan="2" halign="left">mean</th>
      <th colspan="2" halign="left">mean</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th>F</th>
      <th>M</th>
      <th>F</th>
      <th>M</th>
      <th>F</th>
      <th>M</th>
      <th>F</th>
      <th>M</th>
      <th>F</th>
      <th>M</th>
      <th>F</th>
      <th>M</th>
    </tr>
    <tr>
      <th>Medal</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bronze</th>
      <td>69.0</td>
      <td>72.0</td>
      <td>24.710549</td>
      <td>26.332251</td>
      <td>12.0</td>
      <td>10.0</td>
      <td>5.329229</td>
      <td>5.870340</td>
      <td>170.003227</td>
      <td>180.045806</td>
      <td>62.757125</td>
      <td>77.841504</td>
    </tr>
    <tr>
      <th>Gold</th>
      <td>63.0</td>
      <td>64.0</td>
      <td>24.373547</td>
      <td>26.490410</td>
      <td>13.0</td>
      <td>13.0</td>
      <td>5.219615</td>
      <td>5.987807</td>
      <td>170.448727</td>
      <td>180.318906</td>
      <td>63.199349</td>
      <td>78.186505</td>
    </tr>
    <tr>
      <th>Silver</th>
      <td>55.0</td>
      <td>73.0</td>
      <td>24.446683</td>
      <td>26.600132</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>5.253111</td>
      <td>6.098221</td>
      <td>170.233783</td>
      <td>180.053626</td>
      <td>62.866892</td>
      <td>77.960887</td>
    </tr>
  </tbody>
</table>
</div>



## Anomali Tespiti - Aykırı Değerler


```python
def anomaliTespiti(df,ozellik):
    outlier_indices = []
    
    for c in ozellik:
        # 1.çeyrek
        Q1 = np.percentile(df[c],25)
        # 3.çeyrek
        Q3 = np.percentile(df[c],75)
        # IQR = Inter Quartile Range
        IQR = Q3 - Q1
        # aykırı değer için ek adım miktarı
        outlier_step = 1.5 * IQR
        # aykırı değeri be bulunduğu indeksi tespit edelim
        outlier_list_col = df[(df[c] < Q1 - outlier_step) | (df[c] > Q3 + outlier_step)].index
        # tespit edilen indeksleri depolayalım
        outlier_indices.extend(outlier_list_col)
        
    # eşsiz aykırı değerleri bulalım
    outlier_indices = Counter(outlier_indices)
    # eğer bir örnek v adet sütunda farklı ise bunu aykırı kabul edelim
    multiple_outliers = list(i for i, v in outlier_indices.items() if v > 1)
            
    return multiple_outliers

```


```python
veri_anomali = veri.loc[anomaliTespiti(veri,["Age","Weight","Height"])]
veri_anomali.Sport.value_counts()
```




    Basketball        64
    Gymnastics        34
    Handball           6
    Athletics          5
    Sailing            3
    Diving             3
    Shooting           1
    Figure Skating     1
    Wrestling          1
    Name: Sport, dtype: int64




```python
plt.figure()
plt.bar(veri_anomali.Sport.value_counts().index, veri_anomali.Sport.value_counts().values)
plt.xticks(rotation = 30)
plt.title("Anomaliye Ratlanan Spor Branşları")
plt.ylabel("Frekans")
plt.grid(True, alpha = 0.5)
plt.show
```




    <function matplotlib.pyplot.show(close=None, block=None)>




    
![png](output_48_1.png)
    



```python
veri_gym = veri_anomali[veri_anomali.Sport == "Gymnastics"]
veri_gym
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13741</th>
      <td>Oana Mihaela Ban</td>
      <td>F</td>
      <td>18.0</td>
      <td>139.0</td>
      <td>36.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>2004</td>
      <td>Summer</td>
      <td>Athina</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>21260</th>
      <td>Bi Wenjing</td>
      <td>F</td>
      <td>14.0</td>
      <td>142.0</td>
      <td>35.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Uneven Bars</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>23763</th>
      <td>Loredana Boboc</td>
      <td>F</td>
      <td>16.0</td>
      <td>139.0</td>
      <td>32.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>2000</td>
      <td>Summer</td>
      <td>Sydney</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>47452</th>
      <td>Laura Cutina</td>
      <td>F</td>
      <td>15.0</td>
      <td>143.0</td>
      <td>36.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>1984</td>
      <td>Summer</td>
      <td>Los Angeles</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>53751</th>
      <td>Deng Linlin</td>
      <td>F</td>
      <td>16.0</td>
      <td>144.0</td>
      <td>34.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>53759</th>
      <td>Deng Linlin</td>
      <td>F</td>
      <td>20.0</td>
      <td>144.0</td>
      <td>34.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Balance Beam</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>69216</th>
      <td>Mariya Yevgenyevna Filatova (-Kurbatova)</td>
      <td>F</td>
      <td>14.0</td>
      <td>136.0</td>
      <td>30.0</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>1976</td>
      <td>Summer</td>
      <td>Montreal</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>69222</th>
      <td>Mariya Yevgenyevna Filatova (-Kurbatova)</td>
      <td>F</td>
      <td>19.0</td>
      <td>136.0</td>
      <td>30.0</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>69225</th>
      <td>Mariya Yevgenyevna Filatova (-Kurbatova)</td>
      <td>F</td>
      <td>19.0</td>
      <td>136.0</td>
      <td>30.0</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Uneven Bars</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>80497</th>
      <td>Maxi Gnauck</td>
      <td>F</td>
      <td>15.0</td>
      <td>148.0</td>
      <td>33.0</td>
      <td>East Germany</td>
      <td>GDR</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Individual All-Around</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>80498</th>
      <td>Maxi Gnauck</td>
      <td>F</td>
      <td>15.0</td>
      <td>148.0</td>
      <td>33.0</td>
      <td>East Germany</td>
      <td>GDR</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>80499</th>
      <td>Maxi Gnauck</td>
      <td>F</td>
      <td>15.0</td>
      <td>148.0</td>
      <td>33.0</td>
      <td>East Germany</td>
      <td>GDR</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Floor Exercise</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>80501</th>
      <td>Maxi Gnauck</td>
      <td>F</td>
      <td>15.0</td>
      <td>148.0</td>
      <td>33.0</td>
      <td>East Germany</td>
      <td>GDR</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Uneven Bars</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>92622</th>
      <td>He Kexin</td>
      <td>F</td>
      <td>16.0</td>
      <td>142.0</td>
      <td>33.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>92623</th>
      <td>He Kexin</td>
      <td>F</td>
      <td>16.0</td>
      <td>142.0</td>
      <td>33.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Uneven Bars</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>92625</th>
      <td>He Kexin</td>
      <td>F</td>
      <td>20.0</td>
      <td>142.0</td>
      <td>33.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2012</td>
      <td>Summer</td>
      <td>London</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Uneven Bars</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>108408</th>
      <td>Jiang Yuyuan</td>
      <td>F</td>
      <td>16.0</td>
      <td>140.0</td>
      <td>32.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>123021</th>
      <td>Anastasiya Nikolayevna Kolesnikova</td>
      <td>F</td>
      <td>16.0</td>
      <td>147.0</td>
      <td>34.0</td>
      <td>Russia</td>
      <td>RUS</td>
      <td>2000</td>
      <td>Summer</td>
      <td>Sydney</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>129940</th>
      <td>Yevgeniya Petrovna Kuznetsova</td>
      <td>F</td>
      <td>15.0</td>
      <td>143.0</td>
      <td>36.0</td>
      <td>Russia</td>
      <td>RUS</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>133000</th>
      <td>Natlija Laonova (-Kravchenko)</td>
      <td>F</td>
      <td>15.0</td>
      <td>142.0</td>
      <td>36.0</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>138311</th>
      <td>Li Shanshan</td>
      <td>F</td>
      <td>16.0</td>
      <td>145.0</td>
      <td>36.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2008</td>
      <td>Summer</td>
      <td>Beijing</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>143279</th>
      <td>Lu Li</td>
      <td>F</td>
      <td>15.0</td>
      <td>136.0</td>
      <td>30.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Uneven Bars</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>143280</th>
      <td>Lu Li</td>
      <td>F</td>
      <td>15.0</td>
      <td>136.0</td>
      <td>30.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Balance Beam</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>144630</th>
      <td>Oksana Vasilyevna Lyapina</td>
      <td>F</td>
      <td>16.0</td>
      <td>144.0</td>
      <td>33.0</td>
      <td>Russia</td>
      <td>RUS</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>160920</th>
      <td>Dominique Helena Moceanu (-Canales)</td>
      <td>F</td>
      <td>14.0</td>
      <td>139.0</td>
      <td>34.0</td>
      <td>United States</td>
      <td>USA</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>163298</th>
      <td>Patricia Moreno Snchez</td>
      <td>F</td>
      <td>16.0</td>
      <td>143.0</td>
      <td>31.0</td>
      <td>Spain</td>
      <td>ESP</td>
      <td>2004</td>
      <td>Summer</td>
      <td>Athina</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Floor Exercise</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>191397</th>
      <td>Celestina Popa</td>
      <td>F</td>
      <td>18.0</td>
      <td>145.0</td>
      <td>35.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>192136</th>
      <td>Gabriela Potorac</td>
      <td>F</td>
      <td>15.0</td>
      <td>144.0</td>
      <td>35.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>192138</th>
      <td>Gabriela Potorac</td>
      <td>F</td>
      <td>15.0</td>
      <td>144.0</td>
      <td>35.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Horse Vault</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>192140</th>
      <td>Gabriela Potorac</td>
      <td>F</td>
      <td>15.0</td>
      <td>144.0</td>
      <td>35.0</td>
      <td>Romania</td>
      <td>ROU</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Balance Beam</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>217446</th>
      <td>Shang Chunsong</td>
      <td>F</td>
      <td>20.0</td>
      <td>143.0</td>
      <td>34.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>235867</th>
      <td>Tan Jiaxin</td>
      <td>F</td>
      <td>19.0</td>
      <td>148.0</td>
      <td>36.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>256864</th>
      <td>Wang Yan</td>
      <td>F</td>
      <td>16.0</td>
      <td>140.0</td>
      <td>33.0</td>
      <td>China</td>
      <td>CHN</td>
      <td>2016</td>
      <td>Summer</td>
      <td>Rio de Janeiro</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>270182</th>
      <td>Kimberley Lyn "Kim" Zmeskal (-Burdette)</td>
      <td>F</td>
      <td>16.0</td>
      <td>139.0</td>
      <td>36.0</td>
      <td>United States</td>
      <td>USA</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Gymnastics</td>
      <td>Gymnastics Women's Team All-Around</td>
      <td>Bronze</td>
    </tr>
  </tbody>
</table>
</div>




```python
veri_gym.Event.value_counts()
```




    Gymnastics Women's Team All-Around          21
    Gymnastics Women's Uneven Bars               6
    Gymnastics Women's Balance Beam              3
    Gymnastics Women's Floor Exercise            2
    Gymnastics Women's Individual All-Around     1
    Gymnastics Women's Horse Vault               1
    Name: Event, dtype: int64




```python
veri_basketbol = veri_anomali[veri_anomali.Sport == "Basketball"]
veri_basketbol
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8834</th>
      <td>Franjo Arapovi</td>
      <td>M</td>
      <td>23.0</td>
      <td>211.0</td>
      <td>120.0</td>
      <td>Yugoslavia</td>
      <td>YUG</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>8835</th>
      <td>Franjo Arapovi</td>
      <td>M</td>
      <td>27.0</td>
      <td>211.0</td>
      <td>120.0</td>
      <td>Croatia</td>
      <td>CRO</td>
      <td>1992</td>
      <td>Summer</td>
      <td>Barcelona</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>21577</th>
      <td>Oleksandr Mykhailovych Bielostienniy</td>
      <td>M</td>
      <td>21.0</td>
      <td>214.0</td>
      <td>117.0</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>21578</th>
      <td>Oleksandr Mykhailovych Bielostienniy</td>
      <td>M</td>
      <td>29.0</td>
      <td>214.0</td>
      <td>117.0</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>1988</td>
      <td>Summer</td>
      <td>Seoul</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>25598</th>
      <td>Carlos Austin Boozer, Jr.</td>
      <td>M</td>
      <td>22.0</td>
      <td>206.0</td>
      <td>117.0</td>
      <td>United States</td>
      <td>USA</td>
      <td>2004</td>
      <td>Summer</td>
      <td>Athina</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Bronze</td>
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
    </tr>
    <tr>
      <th>270119</th>
      <td>Rajko ii</td>
      <td>M</td>
      <td>21.0</td>
      <td>210.0</td>
      <td>110.0</td>
      <td>Yugoslavia</td>
      <td>YUG</td>
      <td>1976</td>
      <td>Summer</td>
      <td>Montreal</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>270120</th>
      <td>Rajko ii</td>
      <td>M</td>
      <td>25.0</td>
      <td>210.0</td>
      <td>110.0</td>
      <td>Yugoslavia</td>
      <td>YUG</td>
      <td>1980</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>270121</th>
      <td>Rajko ii</td>
      <td>M</td>
      <td>29.0</td>
      <td>210.0</td>
      <td>110.0</td>
      <td>Yugoslavia</td>
      <td>YUG</td>
      <td>1984</td>
      <td>Summer</td>
      <td>Los Angeles</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>270740</th>
      <td>Eurelijus ukauskas</td>
      <td>M</td>
      <td>22.0</td>
      <td>218.0</td>
      <td>115.0</td>
      <td>Lithuania</td>
      <td>LTU</td>
      <td>1996</td>
      <td>Summer</td>
      <td>Atlanta</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>270741</th>
      <td>Eurelijus ukauskas</td>
      <td>M</td>
      <td>27.0</td>
      <td>218.0</td>
      <td>115.0</td>
      <td>Lithuania</td>
      <td>LTU</td>
      <td>2000</td>
      <td>Summer</td>
      <td>Sydney</td>
      <td>Basketball</td>
      <td>Basketball Men's Basketball</td>
      <td>Bronze</td>
    </tr>
  </tbody>
</table>
<p>64 rows × 13 columns</p>
</div>




```python
veri_basketbol.Event.value_counts()
```




    Basketball Men's Basketball      62
    Basketball Women's Basketball     2
    Name: Event, dtype: int64



## Zaman Serilerinde Veri Analizi


```python
veri_zaman = veri.copy()
```


```python
essiz_yıllar = veri_zaman.Year.unique()
essiz_yıllar
```




    array([1900, 1920, 2014, 1948, 1952, 1992, 1994, 2002, 2006, 2008, 1988,
           1996, 1960, 1912, 1956, 2016, 2012, 2000, 2004, 1980, 1984, 1936,
           1906, 1964, 1972, 1924, 1904, 1932, 1928, 1968, 1976, 2010, 1908,
           1998, 1896])




```python
# olimpiyatların yapıldığı yılları sıralayalım
dizili_array = np.sort(essiz_yıllar)
dizili_array
```




    array([1896, 1900, 1904, 1906, 1908, 1912, 1920, 1924, 1928, 1932, 1936,
           1948, 1952, 1956, 1960, 1964, 1968, 1972, 1976, 1980, 1984, 1988,
           1992, 1994, 1996, 1998, 2000, 2002, 2004, 2006, 2008, 2010, 2012,
           2014, 2016])




```python
plt.figure()
plt.scatter(range(len(dizili_array)), dizili_array)
plt.grid(True)
plt.ylabel("Yıllar")
plt.title("Olimpiyatlar Çift Yıllarda Düzenlenir")
plt.show()
```


    
![png](output_57_0.png)
    



```python
# veri içerisinde bulunan yıl değerlerini datetime veri tipine dönüştürelim
tarih_saat_objesi = pd.to_datetime(veri_zaman["Year"], format = '%Y')
print(type(tarih_saat_objesi))
tarih_saat_objesi.head(3)
```

    <class 'pandas.core.series.Series'>





    3    1900-01-01
    37   1920-01-01
    38   1920-01-01
    Name: Year, dtype: datetime64[ns]




```python
veri_zaman["tarih_saat"] = tarih_saat_objesi
veri_zaman.head(3)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Year</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
      <th>tarih_saat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>182.48</td>
      <td>95.62</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>1900</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
      <td>1900-01-01</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>182.01</td>
      <td>76.69</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 200 metres Breaststroke</td>
      <td>Bronze</td>
      <td>1920-01-01</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>177.00</td>
      <td>75.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>1920</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 400 metres Breaststroke</td>
      <td>Bronze</td>
      <td>1920-01-01</td>
    </tr>
  </tbody>
</table>
</div>




```python
# veri_zaman değişkeninin ana indeksini, datetime tipi olan tarih_saat değerine güncelleyelim
veri_zaman = veri_zaman.set_index("tarih_saat")
veri_zaman.drop(["Year"], axis = 1, inplace= True)
veri_zaman
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1900-01-01</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>182.48</td>
      <td>95.62</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>Gold</td>
    </tr>
    <tr>
      <th>1920-01-01</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>182.01</td>
      <td>76.69</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 200 metres Breaststroke</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>1920-01-01</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>177.00</td>
      <td>75.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 400 metres Breaststroke</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>2014-01-01</th>
      <td>Juhamatti Tapio Aaltonen</td>
      <td>M</td>
      <td>28.0</td>
      <td>184.00</td>
      <td>85.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>Winter</td>
      <td>Sochi</td>
      <td>Ice Hockey</td>
      <td>Ice Hockey Men's Ice Hockey</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>1948-01-01</th>
      <td>Paavo Johannes Aaltonen</td>
      <td>M</td>
      <td>28.0</td>
      <td>175.00</td>
      <td>64.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>Summer</td>
      <td>London</td>
      <td>Gymnastics</td>
      <td>Gymnastics Men's Individual All-Around</td>
      <td>Bronze</td>
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
    </tr>
    <tr>
      <th>1956-01-01</th>
      <td>Galina Ivanovna Zybina (-Fyodorova)</td>
      <td>F</td>
      <td>25.0</td>
      <td>168.00</td>
      <td>80.00</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>Summer</td>
      <td>Melbourne</td>
      <td>Athletics</td>
      <td>Athletics Women's Shot Put</td>
      <td>Silver</td>
    </tr>
    <tr>
      <th>1964-01-01</th>
      <td>Galina Ivanovna Zybina (-Fyodorova)</td>
      <td>F</td>
      <td>33.0</td>
      <td>168.00</td>
      <td>80.00</td>
      <td>Soviet Union</td>
      <td>URS</td>
      <td>Summer</td>
      <td>Tokyo</td>
      <td>Athletics</td>
      <td>Athletics Women's Shot Put</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>1980-01-01</th>
      <td>Bogusaw Zych</td>
      <td>M</td>
      <td>28.0</td>
      <td>182.00</td>
      <td>82.00</td>
      <td>Poland</td>
      <td>POL</td>
      <td>Summer</td>
      <td>Moskva</td>
      <td>Fencing</td>
      <td>Fencing Men's Foil, Team</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>2000-01-01</th>
      <td>Olesya Nikolayevna Zykina</td>
      <td>F</td>
      <td>19.0</td>
      <td>171.00</td>
      <td>64.00</td>
      <td>Russia</td>
      <td>RUS</td>
      <td>Summer</td>
      <td>Sydney</td>
      <td>Athletics</td>
      <td>Athletics Women's 4 x 400 metres Relay</td>
      <td>Bronze</td>
    </tr>
    <tr>
      <th>2004-01-01</th>
      <td>Olesya Nikolayevna Zykina</td>
      <td>F</td>
      <td>23.0</td>
      <td>171.00</td>
      <td>64.00</td>
      <td>Russia</td>
      <td>RUS</td>
      <td>Summer</td>
      <td>Athina</td>
      <td>Athletics</td>
      <td>Athletics Women's 4 x 400 metres Relay</td>
      <td>Silver</td>
    </tr>
  </tbody>
</table>
<p>39783 rows × 12 columns</p>
</div>



## Yıllara Göre Ortalama Yaş, Boy ve Kilo Değişimi


```python
periyodik_veri = veri_zaman.resample("2A").mean() # 2 yıllık periyotlar halinde ortalama değerleri alır
periyodik_veri.head()
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1896-12-31</th>
      <td>23.905734</td>
      <td>174.280350</td>
      <td>72.734056</td>
    </tr>
    <tr>
      <th>1898-12-31</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1900-12-31</th>
      <td>27.786689</td>
      <td>177.882301</td>
      <td>74.979950</td>
    </tr>
    <tr>
      <th>1902-12-31</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1904-12-31</th>
      <td>26.363868</td>
      <td>177.241091</td>
      <td>74.330823</td>
    </tr>
  </tbody>
</table>
</div>




```python
# kayıp verileri çıkaralım 
periyodik_veri.dropna(axis = 0, inplace=True)
periyodik_veri.head()
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1896-12-31</th>
      <td>23.905734</td>
      <td>174.280350</td>
      <td>72.734056</td>
    </tr>
    <tr>
      <th>1900-12-31</th>
      <td>27.786689</td>
      <td>177.882301</td>
      <td>74.979950</td>
    </tr>
    <tr>
      <th>1904-12-31</th>
      <td>26.363868</td>
      <td>177.241091</td>
      <td>74.330823</td>
    </tr>
    <tr>
      <th>1906-12-31</th>
      <td>26.479389</td>
      <td>176.347576</td>
      <td>74.072183</td>
    </tr>
    <tr>
      <th>1908-12-31</th>
      <td>27.566739</td>
      <td>176.662419</td>
      <td>73.721107</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure()
periyodik_veri.plot()
plt.title("Yıllara Göre Ortalama Yaş, Boy ve Ağırlık Değişimi")
plt.xlabel("Yıl")
plt.grid(True)
plt.show()
```


    <Figure size 432x288 with 0 Axes>



    
![png](output_64_1.png)
    


## Yıllara Göre Madalya Sayıları


```python
veri_zaman = pd.get_dummies(veri_zaman, columns=["Medal"])
veri_zaman.head(3)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1900-01-01</th>
      <td>Edgar Lindenau Aabye</td>
      <td>M</td>
      <td>34.0</td>
      <td>182.48</td>
      <td>95.62</td>
      <td>Denmark/Sweden</td>
      <td>DEN</td>
      <td>Summer</td>
      <td>Paris</td>
      <td>Tug-Of-War</td>
      <td>Tug-Of-War Men's Tug-Of-War</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1920-01-01</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>182.01</td>
      <td>76.69</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 200 metres Breaststroke</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1920-01-01</th>
      <td>Arvo Ossian Aaltonen</td>
      <td>M</td>
      <td>30.0</td>
      <td>177.00</td>
      <td>75.00</td>
      <td>Finland</td>
      <td>FIN</td>
      <td>Summer</td>
      <td>Antwerpen</td>
      <td>Swimming</td>
      <td>Swimming Men's 400 metres Breaststroke</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
periyodik_veri = veri_zaman.resample("2A").sum()
periyodik_veri.head()
periyodik_veri = periyodik_veri[~(periyodik_veri == 0).any(axis=1)]
periyodik_veri.tail()
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2008-12-31</th>
      <td>53344.0</td>
      <td>365157.91</td>
      <td>152090.92</td>
      <td>710.0</td>
      <td>671.0</td>
      <td>667.0</td>
    </tr>
    <tr>
      <th>2010-12-31</th>
      <td>13896.0</td>
      <td>91395.00</td>
      <td>37877.12</td>
      <td>171.0</td>
      <td>174.0</td>
      <td>175.0</td>
    </tr>
    <tr>
      <th>2012-12-31</th>
      <td>50595.0</td>
      <td>346091.47</td>
      <td>143102.94</td>
      <td>679.0</td>
      <td>632.0</td>
      <td>630.0</td>
    </tr>
    <tr>
      <th>2014-12-31</th>
      <td>15907.0</td>
      <td>104686.00</td>
      <td>42838.63</td>
      <td>198.0</td>
      <td>202.0</td>
      <td>197.0</td>
    </tr>
    <tr>
      <th>2016-12-31</th>
      <td>53256.0</td>
      <td>360846.03</td>
      <td>149628.71</td>
      <td>703.0</td>
      <td>665.0</td>
      <td>655.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure()
periyodik_veri.loc[:,["Medal_Bronze","Medal_Silver","Medal_Gold"]].plot()
plt.title("Yıllara göre madalya sayıları")
plt.ylabel("Madalya Sayısı")
plt.xlabel("Yıl")
plt.grid(True)
plt.show()
```


    <Figure size 432x288 with 0 Axes>



    
![png](output_68_1.png)
    


## Yıllara ve Sezonlara Göre Madalya Sayıları


```python
yaz = veri_zaman[veri_zaman.Season == "Summer"]
kis = veri_zaman[veri_zaman.Season == "Winter"]
kis.tail(3)
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
      <th>Name</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Team</th>
      <th>NOC</th>
      <th>Season</th>
      <th>City</th>
      <th>Sport</th>
      <th>Event</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1988-01-01</th>
      <td>Pirmin Zurbriggen</td>
      <td>M</td>
      <td>25.0</td>
      <td>183.0</td>
      <td>83.0</td>
      <td>Switzerland</td>
      <td>SUI</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Alpine Skiing</td>
      <td>Alpine Skiing Men's Downhill</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1988-01-01</th>
      <td>Pirmin Zurbriggen</td>
      <td>M</td>
      <td>25.0</td>
      <td>183.0</td>
      <td>83.0</td>
      <td>Switzerland</td>
      <td>SUI</td>
      <td>Winter</td>
      <td>Calgary</td>
      <td>Alpine Skiing</td>
      <td>Alpine Skiing Men's Giant Slalom</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2010-01-01</th>
      <td>Silvan Zurbriggen</td>
      <td>M</td>
      <td>28.0</td>
      <td>185.0</td>
      <td>94.0</td>
      <td>Switzerland</td>
      <td>SUI</td>
      <td>Winter</td>
      <td>Vancouver</td>
      <td>Alpine Skiing</td>
      <td>Alpine Skiing Men's Combined</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
periyodik_veri_kis = kis.resample("A").sum()
periyodik_veri_kis = periyodik_veri_kis[~(periyodik_veri_kis == 0).any(axis=1)]
periyodik_veri_kis.head()
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1924-12-31</th>
      <td>3919.60</td>
      <td>22867.28</td>
      <td>9703.58</td>
      <td>37</td>
      <td>55</td>
      <td>38</td>
    </tr>
    <tr>
      <th>1928-12-31</th>
      <td>2265.56</td>
      <td>15745.75</td>
      <td>6862.50</td>
      <td>31</td>
      <td>30</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1932-12-31</th>
      <td>2431.00</td>
      <td>16357.71</td>
      <td>7067.76</td>
      <td>28</td>
      <td>32</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1936-12-31</th>
      <td>2742.00</td>
      <td>19123.20</td>
      <td>8101.88</td>
      <td>35</td>
      <td>36</td>
      <td>37</td>
    </tr>
    <tr>
      <th>1948-12-31</th>
      <td>3643.00</td>
      <td>23942.51</td>
      <td>10375.92</td>
      <td>46</td>
      <td>41</td>
      <td>48</td>
    </tr>
  </tbody>
</table>
</div>




```python
periyodik_veri_yaz = yaz.resample("A").sum()
periyodik_veri_yaz = periyodik_veri_yaz[~(periyodik_veri_yaz == 0).any(axis=1)]
periyodik_veri_yaz.head()
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
      <th>Age</th>
      <th>Height</th>
      <th>Weight</th>
      <th>Medal_Bronze</th>
      <th>Medal_Gold</th>
      <th>Medal_Silver</th>
    </tr>
    <tr>
      <th>tarih_saat</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1896-12-31</th>
      <td>3418.52</td>
      <td>24922.09</td>
      <td>10400.97</td>
      <td>38.0</td>
      <td>62.0</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>1900-12-31</th>
      <td>16783.16</td>
      <td>107440.91</td>
      <td>45287.89</td>
      <td>175.0</td>
      <td>201.0</td>
      <td>228.0</td>
    </tr>
    <tr>
      <th>1904-12-31</th>
      <td>12812.84</td>
      <td>86139.17</td>
      <td>36124.78</td>
      <td>150.0</td>
      <td>173.0</td>
      <td>163.0</td>
    </tr>
    <tr>
      <th>1906-12-31</th>
      <td>12127.56</td>
      <td>80767.19</td>
      <td>33925.06</td>
      <td>145.0</td>
      <td>157.0</td>
      <td>156.0</td>
    </tr>
    <tr>
      <th>1908-12-31</th>
      <td>22907.96</td>
      <td>146806.47</td>
      <td>61262.24</td>
      <td>256.0</td>
      <td>294.0</td>
      <td>281.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure()
periyodik_veri_yaz.loc[:,["Medal_Bronze","Medal_Silver","Medal_Gold"]].plot()
plt.title("Yıllara göre madalya sayıları - yaz sezonu")
plt.ylabel("Madalya Sayısı")
plt.xlabel("Yıl")
plt.grid(True)
plt.show()

plt.figure()
periyodik_veri_kis.loc[:,["Medal_Bronze","Medal_Silver","Medal_Gold"]].plot()
plt.title("Yıllara göre madalya sayıları - kış sezonu")
plt.ylabel("Madalya Sayısı")
plt.xlabel("Yıl")
plt.grid(True)
plt.show()
```


    <Figure size 432x288 with 0 Axes>



    
![png](output_73_1.png)
    



    <Figure size 432x288 with 0 Axes>



    
![png](output_73_3.png)
    



```python

```
