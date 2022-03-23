---
title: Perfumes Recommendation System
author: Rawan ALQarni
date: '2022-03-25'
--- 

Perfumes recommender is a recommendation system for perfumes. As we all know recommendation systems are popular these days and used by a lot of companies in order to understand their customer and provide them with a better service. This project aim to find similar perfumes based on perfumes features.

### EDA 

#### Import Packages

```python
import pandas as pd 
import numpy as np
import matplotlib.pyplot as plt
```

#### Read Dataset:
- Perfumes Dataset: 
This dataset contain Perfumes data: Perfumes_Name, Brand, Describtion, Price, Gender, Product_type, Fragrance_Family, Character, Average Rate, Ingredients, Top, Middle and Base Notes, image_URL.

- Reviewa Dataset: 
This dataset contain User nickname, Perfume_name, Brand, Fragrance_Family, Overall_rating, User_rating, Revirews, Date.   


```python
Perfumes_df = pd.read_csv("C:/Users/hmq4/OneDrive/Desktop/Data Science- Misk/chrome/Misk_DSI_Capstone_Project/Dataset/Datasets/Perfume_Dataset.csv")
Reviews_df = pd.read_csv("C:/Users/hmq4/OneDrive/Desktop/Data Science- Misk/chrome/Misk_DSI_Capstone_Project/Dataset/Datasets/Reviews_Dataset.csv")
```

#### Data Dictionary:
<table>
<tr>
<th> Column </th>
<th> Description </th>
</tr>

<tr>
    <td> Name </td>
    <td> Full name of the perfume </td>
</tr>


<tr>
    <td> Price </td>
    <td> The price is in SAR and it is an object type not a float </td>
</tr>  


<tr>
    <td> Description </td>
    <td> The description consist of a perfume description as well as a small description about the brand </td>
</tr>


<tr>
    <td> Rate </td>
    <td> This represent the average rating for the perfume by the users on a scale 0-5</td>
</tr>


<tr>
    <td> Rating_count </td>
    <td> This represent how many users have rated the perfume </td>
</tr>


<tr>
    <td> image </td>
    <td> This contain the image url of the perfume </td>
</tr>

<tr>
    <td> Brand </td>
    <td> The dataset contains 484 brands </td>
</tr>

<tr>
    <td> Gender </td>
    <td> The gender as Wonem, Men, Home, Unisex </td>
</tr>

<tr>
    <td> Product_Type </td>
    <td> This specify the product type such as perfume, perfume set, perfume oil, candle </td>
</tr>

<tr>
    <td> Character </td>
    <td> this specify a character for each perfume such as Romantic,Charismatic. </td>
</tr>

<tr>
    <td> Fragrance_Family </td>
    <td> This reperesent the class of the perfume such as floral, woody </td>
</tr>

<tr>
    <td> Size </td>
    <td> Most Perfume measured in ml, however the dataset contains other sizes: g </td>
</tr>

<tr>
    <td> Year </td>
    <td> This represent the year the perfume have lunched </td>
</tr>

<tr>
    <td> Ingredients </td>
    <td> This contain all the perfume Ingredients </td>
</tr>

<tr>
    <td> Concentration</td>
    <td> This represent how strong the perfume is such as Parfum which is the strongest </td>
</tr>

<tr>
    <td> Top_note </td>
    <td> The Top Note is the note Ingredients, this note is the light note </td>
</tr>

<tr>
    <td> Middle_note </td>
    <td> The Middle note is the core and very essence of the of the perfume</td>
</tr>

<tr>
    <td> Base_note </td>
    <td> The base notes of the perfume give it depth, richness (the foundation of the perfume)</td>
</tr>

</table>


### 1. Perfumes Dataset Cleaning 

Summary of the Cleaning step:
1. Remove duplicate
2. Change price datatype and remove SAR
3. Change Rate,Rating_count datatype to float 
4. Check all the columns values,so they don't have any wrong values >> resulted from web scraping
5. Remove the size from Name  
6. Handle null values 
7. A lot of manual cleaning to resolve inconsistency 


```python
Perfumes_df.shape
```




    (6237, 19)




```python
# Checking for duplicate 
print(f"The DataFrame have {Perfumes_df.duplicated().sum()} duplicate")
# remove duplicate
Perfumes_df = Perfumes_df.drop_duplicates()
```

    The DataFrame have 25 duplicate



```python
Perfumes_df.shape
```




    (6212, 19)




```python
Perfumes_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 6212 entries, 0 to 6236
    Data columns (total 19 columns):
     #   Column            Non-Null Count  Dtype 
    ---  ------            --------------  ----- 
     0   Name              6212 non-null   object
     1   Price             6212 non-null   object
     2   Description       6103 non-null   object
     3   Rate              6103 non-null   object
     4   Rating_count      6098 non-null   object
     5   Details           6097 non-null   object
     6   image             6097 non-null   object
     7   Brand             5808 non-null   object
     8   Gender            5895 non-null   object
     9   Product_Type      5594 non-null   object
     10  Character         5347 non-null   object
     11  Fragrance_Family  5582 non-null   object
     12  Size              5571 non-null   object
     13  Year              4211 non-null   object
     14  Ingredients       5454 non-null   object
     15  Concentration     4855 non-null   object
     16  Top_note          5283 non-null   object
     17  Middle_note       4977 non-null   object
     18  Base_note         4939 non-null   object
    dtypes: object(19)
    memory usage: 970.6+ KB


After manual cleaning I created a new csv file and copy the data to it, unfortunately the dataset size get reduced 


```python
final_df = pd.read_csv("C:/Users/hmq4/OneDrive/Desktop/Data Science- Misk/chrome/Misk_DSI_Capstone_Project/Dataset/Datasets/Dataset.csv")
```


```python
final_df.shape
```




    (4239, 18)




```python
# Checking for duplicate 
print(f"The DataFrame have {final_df.duplicated().sum()} duplicate")
# remove duplicate
final_df = final_df.drop_duplicates()
```

    The DataFrame have 0 duplicate



```python
final_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 4239 entries, 0 to 4238
    Data columns (total 18 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   Name              4239 non-null   object 
     1   Price             4239 non-null   object 
     2   Description       4239 non-null   object 
     3   Rate              4239 non-null   object 
     4   Rating_count      4239 non-null   object 
     5   image             4239 non-null   object 
     6   Brand             4239 non-null   object 
     7   Gender            4239 non-null   object 
     8   Product_Type      4239 non-null   object 
     9   Character_x       4239 non-null   object 
     10  Fragrance_Family  4219 non-null   object 
     11  Size              4238 non-null   object 
     12  Year              4218 non-null   float64
     13  Ingredients       4239 non-null   object 
     14  Concentration     4225 non-null   object 
     15  Top_note          4239 non-null   object 
     16  Middle_note       4233 non-null   object 
     17  Base_note         4224 non-null   object 
    dtypes: float64(1), object(17)
    memory usage: 629.2+ KB



```python
final_df.head()
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
      <th>Price</th>
      <th>Description</th>
      <th>Rate</th>
      <th>Rating_count</th>
      <th>image</th>
      <th>Brand</th>
      <th>Gender</th>
      <th>Product_Type</th>
      <th>Character_x</th>
      <th>Fragrance_Family</th>
      <th>Size</th>
      <th>Year</th>
      <th>Ingredients</th>
      <th>Concentration</th>
      <th>Top_note</th>
      <th>Middle_note</th>
      <th>Base_note</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Dolce &amp; Gabanna L'imperatrice 3 Pour Femme</td>
      <td>199</td>
      <td>Perfume for the energetic woman who is a hero ...</td>
      <td>5</td>
      <td>6 Rating</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Dolce&amp;Gabbana</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Floral</td>
      <td>100 ml</td>
      <td>2009.0</td>
      <td>Watermerlon, Kiwi, Pink Cyclamen, Musk, Pink P...</td>
      <td>Eau de Toilette</td>
      <td>Pink pepper, kiwi, rhubarb</td>
      <td>jasmine, cyclamen, watermelon</td>
      <td>musk, sandalwood, lemon trees.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Roberto Cavalli Paradiso</td>
      <td>169</td>
      <td>Woody floral fragrance, a subtle aroma that ma...</td>
      <td>4.95</td>
      <td>17 Rating</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Roberto Cavalli</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Woody</td>
      <td>50 ml</td>
      <td>2015.0</td>
      <td>Citrus, mandarin, bergamot, jasmine, pine, cyp...</td>
      <td>Eau de Parfum</td>
      <td>Citruses , Mandarin , Bergamot</td>
      <td>Jasmine</td>
      <td>Cypress, Parasol pine, Pink laurel</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Yves Saint Laurent Libre</td>
      <td>389</td>
      <td>This perfume is a reflection of Freedom, speci...</td>
      <td>5</td>
      <td>3 Rating</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Yves Saint Laurent</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Floral</td>
      <td>90 ml</td>
      <td>2019.0</td>
      <td>Mandarin Orange, lavendar, black currant, peti...</td>
      <td>Eau de Parfum</td>
      <td>Mandarin Orange, Lavendar, Black Currant, Peti...</td>
      <td>Jasmine, Orange Blossom</td>
      <td>Vanilla, Cedar, Musk, Ambergris</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mancera Red Tobacco</td>
      <td>499</td>
      <td>Mancera Red Tobacco is the new oriental, woody...</td>
      <td>4.38</td>
      <td>8 Rating</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Mancera</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Oriental</td>
      <td>120 ml</td>
      <td>2017.0</td>
      <td>Saffron, Cinnamon, Incense, Nutmeg, White Peac...</td>
      <td>Eau de Parfum</td>
      <td>Saffron, Cinnamon, Incense, Nutmeg, White peac...</td>
      <td>Patchouli, Jasmine</td>
      <td>Tobacco, Amber, Woody notes, Vetiver, Vanilla,...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Giorgio Armani Emporio Armani Stronger With Yo...</td>
      <td>399</td>
      <td>for every romantic gentle man, This strong fas...</td>
      <td>5</td>
      <td>3 Rating</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Giorgio Armani</td>
      <td>Men</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Aromatic</td>
      <td>100 ml</td>
      <td>2019.0</td>
      <td>Spices, Violet, Lavender, Sweet Toffee, Carame...</td>
      <td>Eau de Parfum</td>
      <td>Pink pepper, Juniper, Violet leaf</td>
      <td>Lavender, Sage, Toffee, Cinnamon</td>
      <td>Tonka bean, Suede, Amber, Vanilla</td>
    </tr>
  </tbody>
</table>
</div>




```python
final_df['Name'] = final_df.Name.str.split('-').str[0]
final_df['Name'] = final_df.Name.str.split('(').str[0]
```


```python
# remove SAR from price column 
final_df['Price'] = final_df.Price.str.split(' ').str[0]
```


```python
# converting price to numric value 
final_df.Price = pd.to_numeric(final_df.Price)
```


```python
# removing "Rating" in Rating_count column
final_df['Rating_count'] = final_df.Rating_count.str.split(' ').str[0]
```

In Rating and Rating_cont columns "none" represent no rating, 0 lowest score and 5 highest score 
Thus, I will keep Rate and Rating_count as it is, so I can know if no rating which majority of the perfume have no rating


```python
final_df.Rate.value_counts()
```




    none    3832
    5        299
    4         34
    4.5       22
    3          9
    3.5        5
    1          5
    4.75       5
    4.25       4
    4.6        4
    4.83       4
    4.67       3
    3.67       3
    4.88       2
    4.95       1
    4.8        1
    4.79       1
    4.38       1
    4.42       1
    4.93       1
    4.89       1
    4.92       1
    Name: Rate, dtype: int64



### 2. EDA
Let's Discover the dataset scraped from Golden Scent Website 


```python
final_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 4239 entries, 0 to 4238
    Data columns (total 18 columns):
     #   Column            Non-Null Count  Dtype  
    ---  ------            --------------  -----  
     0   Name              4239 non-null   object 
     1   Price             4239 non-null   int64  
     2   Description       4239 non-null   object 
     3   Rate              4239 non-null   object 
     4   Rating_count      4239 non-null   object 
     5   image             4239 non-null   object 
     6   Brand             4239 non-null   object 
     7   Gender            4239 non-null   object 
     8   Product_Type      4239 non-null   object 
     9   Character_x       4239 non-null   object 
     10  Fragrance_Family  4219 non-null   object 
     11  Size              4238 non-null   object 
     12  Year              4218 non-null   float64
     13  Ingredients       4239 non-null   object 
     14  Concentration     4225 non-null   object 
     15  Top_note          4239 non-null   object 
     16  Middle_note       4233 non-null   object 
     17  Base_note         4224 non-null   object 
    dtypes: float64(1), int64(1), object(16)
    memory usage: 629.2+ KB



```python
print(f'This dataset contains: {final_df.shape}')
```

    This dataset contains: (4239, 18)


##### How many Brand in the Dataset?


```python
print(f'This dataset contain: {final_df.Brand.nunique()}')
```

    This dataset contain: 385



```python
top_brand = final_df.groupby('Brand').agg({'Price':'max'}).sort_values(by=['Price'], ascending=False)
top_brand.head()
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
      <th>Price</th>
    </tr>
    <tr>
      <th>Brand</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bvlgari</th>
      <td>8193</td>
    </tr>
    <tr>
      <th>Creed</th>
      <td>6285</td>
    </tr>
    <tr>
      <th>Xerjoff</th>
      <td>5750</td>
    </tr>
    <tr>
      <th>Boadicea</th>
      <td>3763</td>
    </tr>
    <tr>
      <th>Roja</th>
      <td>3260</td>
    </tr>
  </tbody>
</table>
</div>



##### What the Perfumes Price range in Dataset?


```python
print(f'Highest Price : {final_df.Price.max()},   Lowest Price : {final_df.Price.min()}')
```

    Highest Price : 8193,   Lowest Price : 18



```python
final_df.Price.value_counts()
```




    119     120
    295     108
    99       53
    199      48
    240      38
           ... 
    471       1
    1192      1
    621       1
    526       1
    2362      1
    Name: Price, Length: 876, dtype: int64




```python
import matplotlib.pyplot as plt
import seaborn as sns 
fig = plt.figure(figsize = (10, 5))
 
# creating the bar plot
sns.histplot(final_df.Price, color ='#AF7AC5')
 
plt.xlabel("Prices")
plt.ylabel("No. of Perfumes")
plt.title("Price Ranges")
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_33_0.png)
    


As the plot shows the majority of perfumes prices are below 1000 SAR and we get less and less as the price get higher   

#### How many perfumes have Rate values and what the Rating distribution?


```python
final_df.Rate.value_counts()
```




    none    3832
    5        299
    4         34
    4.5       22
    3          9
    3.5        5
    1          5
    4.75       5
    4.25       4
    4.6        4
    4.83       4
    4.67       3
    3.67       3
    4.88       2
    4.95       1
    4.8        1
    4.79       1
    4.38       1
    4.42       1
    4.93       1
    4.89       1
    4.92       1
    Name: Rate, dtype: int64




```python
final_df.Rating_count.value_counts()
```




    none    3832
    1        248
    2         83
    3         38
    4         17
    5          8
    7          4
    6          3
    8          2
    17         1
    12         1
    18         1
    13         1
    Name: Rating_count, dtype: int64




```python
import matplotlib.pyplot as plt
import seaborn as sns 
fig = plt.figure(figsize = (10, 5))
sns.cubehelix_palette(start=.5, rot=-.5, as_cmap=True)
# creating the bar plot
plt.hist(final_df.Rate, color ='#AF7AC5')
 
plt.xlabel("Rates")
plt.ylabel("No. of Perfumes")
plt.title("Rate Ranges")
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_38_0.png)
    


As shown above I don't have a lot of rating data, thus Iam going to choose a content-based model that focuses on perfume features 

#### What the format of the Description column? 


```python
final_df.Description.value_counts()
```




    Unknown                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         34
    It's a magnificent blend of sweet and smoky notes that highlights its Middle Eastern roots with warm amber, Taif rose, oud, and exotic incense leading the way for a divine blend of seductive scents. Hints of blackcurrant, raspberry, ylang-ylang, Bulgarian rose, and rich white jasmine all incorporate their own fruity and floral accords, while the addition of pink pepper, sandalwood, patchouli, musk, and leather encompass the senses with a spicy and earthy final touch.\nAbout the brand:\nThe Estee Lauder Companies Inc. is one of the world's leading manufacturers and marketers of quality skin care, makeup, fragrance and hair care products. The company began in 1946 when EstÃƒÂ©e Lauder and her husband Joseph Lauder began producing cosmetics in New York City. They first carried only four products: Cleansing Oil, Skin Lotion, Super Rich All purpose Creme, and Creme Pack.                                                                                                                                                                                                                                                                                                   4
    A refreshing fragrance for women seeking the highest levels of charming feminine beauty.\nIt has notes of rose, lychee and attractive amber.\nIt adds elegance to your look.\nIt helps you keep your look gorgeous for long hours.\nAbout the brand:\nJeanne Arthes is a French brand with more than 40 years of experience and creativity in producing luxurious fragrances, which are widely sold across the world.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            4
    About the brand:\nFounded in 1925 by Hugo Boss for clothing launching purpose, Hugo Boss is a German luxury fashion house. As of 2016, Hugo Boss has 1,100 retail stores worldwide. Hugo Boss has emerged as probably one of the most famous brands in the manufacturing and selling of fashion products. With a strong emphasis on both masculine and feminine products, you simply cannot help but fall in love with their perfumes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           4
    A citrus fresh scent launched in 2011, Pure White Cologne, by Creed, is part of the Royal Exclusives Line created to celebrate the houseÃ¢â‚¬â„¢s 250th anniversary. This fragrance is also known as Original Cologne. Top notes of this scent are all citrus, with bittersweet grapefruit, tart lemon, and tangy bergamot. Middle heart notes are pear, neroli, and galbanum, followed by the interesting base note mix of rice, ambergris, white musk, and powdery notes. Best worn as a daytime cologne in the warmer months. A must-have for your perfume vanity!\nAbout the brand:\nA British multinational perfume house, Creed started off as a tailoring house in the 1700s. In the 1900s, Olivier Creed released the brand's first eponymously named fragrance, a traditional Eau de Cologne with a matching aftershave, marking Creed's first official foray into the world of fragrances. Today, the brand is considered as one of the most influential fragrance brands in the world.                                                                                                                                                                                                                4
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    ..
    Le Male Essence is an oriental fragrance that mixes many different ingredients to give you a bright fusion that increases your charisma. The fragrance combines bergamot, cardamom, Georgy wood, osmanthus, leather, vanilla and Tonka.\nAbout the brand:\nJean Paul Gaultier is a French fashion designer whose first collection was presented in 1976. The first perfume of the Gaultier house was launched in 1993, and now has 125 perfumes in his fragrance base.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           1
    This strong wonderful scent is the newest aromatic edition for men by Giorgio Armani.It is for the gentle charismatic man who lives in the present and like to feel modern a the time.It is a lovely mix of two fragrances, one perfectly feminine, the other strongly masculine. Use it and never regret.\nAbout the brand:\nGirogio Armani has no competition when it comes to quality fragrances and perfumes for men and women. With an annual turnover of $1.6 billion as of 2017, Armani specializes in ready-to-wear, accessories, glasses, cosmetics, and perfumes. It is one of the most popular brands that have stopped using animal fur in their products.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           1
    Korloff In White Intense is a perfume for Men with an Aromatic Fougere fragrance, it was launched in 2010. The top notes are cardamom, bergamot and violet leaf; middle notes are lavender, African orange flower, and clary sage; base notes are cashmere, Virginia cedar, musk, and tonka bean.\nAbout the brand:\nThe brand Korloff Paris was found in 1978 by Daniel Paillasseur, who named it after a luxurious and legendary diamond. His unique and innovative creations earned him reputation and international recognition even before Korloff was found. The first fragrance in the house was simply named Korloff, a floral.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          1
    Introduced in 2005, Dior Homme by Christian Dior is an aromatic, woody floral fragrance for men, and the first of its kind to use iris as part of its scent makeup. Sage, lavender, and bergamot are the top notes of this masculine perfume, with middle notes of iris, cacao, cardamom, and amber. Bookended with base notes of Tahitian vetiver, leather, and patchouli, this perfume formulated by Olivier Polge is elegant, sexy, and unique, especially for winter and evening wear. Dior Homme is worn as a sleek modern suit, leaving behind an immediately recognizable woody and spicy trail of elegance.\nAbout the brand:\nFounded in 1946 by designer Christian Dior, Dior is a pillar in the world of leather goods, fashion accessories, footwear, jewelry, timepieces, fragrance, makeup and skin care products, maintaining a long tradition as a creator of haute-couture fashion. In 1947, the group's first fragrance, Miss Dior, took the world by storm. Since that fateful day, Dior fragrances have truly made their mark thanks to their scope of influence and the use of the noblest of materials, boldly and passionately creating high-quality, unique and timeless fragrances.     1
    Al Athal is an aromatic resin perfume bridging and celebrating the bonds between the generations of the region. Love, respect and innovative traditions are carried in the scent of Al Athal. Spices, flowers and Frankincence at the top lay on an olfactive bed of Amber and Musk. Al Athal Oud takes the beautiful initial scent, and adds pure precious Indian Oud Oil, to make the scent deeper and warmer. Ghawali Concentrated Perfumes are a 100% oil-based fragrance, ideal for your pulse points and for layering with our perfumes.\nAbout the brand:\nGhawali was founded in 2016 to reinvent the oriental universe into a luxurious modern oriental niche experience. At Ghawali, the storytelling heritage of fragrances is our core, crafting personalized and luxury scents is our essence while building and embracing the community is our strong passion. We aim to create scented journeys through our unique offering and embracing our guests quest for a personalized and styled scented journey. Ghawali is our celebration and love for the scented journeys of the GCC.                                                                                                                1
    Name: Description, Length: 3659, dtype: int64




```python
pd.set_option("display.max_colwidth", -1)
final_df.Description.head(1)
```

    C:\Users\hmq4\AppData\Local\Temp\ipykernel_8864\4272716560.py:1: FutureWarning: Passing a negative integer is deprecated in version 1.0 and will not be supported in future version. Instead, use None to not limit the column width.
      pd.set_option("display.max_colwidth", -1)





    0    Perfume for the energetic woman who is a hero in her movie in life every day! It keeps you vibrant and sparkling with irresistible attractiveness, a combination of kiwi, lemon and sandalwood.\nAbout the brand:\nDolce & Gabbana was established in 1985 by two Italian fashion designers. Dolce & Gabbana has not only managed to build a loyal customer base in such a short span of time but it also partnered with Motorola and Sony Ericson for multiple campaigns. D&G has also partnered with football clubs namely AC Milan and Chelsea FC.
    Name: Description, dtype: object



As shown above Description is not unique for all perfume, some brand provide static description for all there perfume and specify perfume features in the following data. 
In addition, Descriptions are a bit long and contains "About Brand:" section which is not our goal,and will be repeated in all perfumes from a specific brand >> thus I will substract about brand part from the description. 


```python
final_df['Description'] = final_df['Description'].str.split("\nAbout the brand:\n").str[0]
pd.set_option("display.max_colwidth", -1)
final_df.Description.head(1)
```

    C:\Users\hmq4\AppData\Local\Temp\ipykernel_8864\2844140122.py:2: FutureWarning: Passing a negative integer is deprecated in version 1.0 and will not be supported in future version. Instead, use None to not limit the column width.
      pd.set_option("display.max_colwidth", -1)





    0    Perfume for the energetic woman who is a hero in her movie in life every day! It keeps you vibrant and sparkling with irresistible attractiveness, a combination of kiwi, lemon and sandalwood.
    Name: Description, dtype: object



#### What perfume Sizes included in the dataset and what is the measures?


```python
final_df.Size.value_counts()
```




    100 ml     2233
    50 ml      685 
    75 ml      259 
    55 ml      147 
    125 ml     117 
              ...  
    251 ml     1   
    18 ml      1   
    75ml       1   
    100 g      1   
    100 ml     1   
    Name: Size, Length: 73, dtype: int64



The majority is measured in ml, however we have grams as well 

#### What are the gender in the dataset and what is the distribution for each?


```python
final_df.Gender.value_counts()
```




    Women     2167
    Unisex    1095
    Men       862 
    Home      115 
    Name: Gender, dtype: int64



#### What the percentage of each gender in the dataset?


```python
total = len(final_df.index)
women = (len(final_df[final_df['Gender'] == 'Women'].index)/total)*100
men = (len(final_df[final_df['Gender'] == 'Men'].index)/total)*100
home = (len(final_df[final_df['Gender'] == 'Home'].index)/total)*100
unisex = (len(final_df[final_df['Gender'] == 'Unisex'].index)/total)*100
```


```python
colors = sns.cubehelix_palette()[0:5]
fig = plt.figure(figsize = (10, 5))
data = [women, men, unisex, home]
labels = ['Women', 'Men', 'Unisex', 'Home']

fig.patch.set_facecolor('white')
#create pie chart
plt.pie(data, labels= labels , colors = colors, autopct='%.0f%%')
plt.show()
```


    
![png](output_52_0.png)
    


#### What product type included in the dataset ? 


```python
print(f'This dataset consist of : {final_df.Product_Type.nunique()} different product type')
```

    This dataset consist of : 15 different product type



```python
final_df.Product_Type.value_counts()
```




    Perfume               3530
    Perfume Set           357 
    Perfume Oil           72  
    Room Spray            62  
    Hair Mist             62  
    Body Mist             43  
    Bakhoor               36  
    Oud                   28  
    Hair & Body Mist      15  
    Diffuser              15  
    Candle                8   
    Bundles               5   
    Body Lotion           4   
    Perfume               1   
    Home Fragrance Set    1   
    Name: Product_Type, dtype: int64



Note: This Product types are after cleaning, before I found some product type that should be excluded like makeup, shower gel, Deodorant


```python
import matplotlib.pyplot as plt
import seaborn as sns 
fig = plt.figure(figsize = (18, 8))
# creating the bar plot
#sns.histplot(final_df.Product_Type, color='final_df.Product_Type')
colors = sns.cubehelix_palette()
sns.histplot(final_df, x="Product_Type", color="#AF7AC5")
plt.xlabel("Product Type")
plt.ylabel("No. of Product Type")
plt.title("Different Product Type")
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_57_0.png)
    


As Shown in the plot Perfumes have the majority as expected  

#### How many fragrance family and which one have the highest count in the dataset? 


```python
print(f'This dataset consist of : {final_df.Fragrance_Family.nunique()} different product type')
print(f'They are : {final_df.Fragrance_Family.unique()}')
```

    This dataset consist of : 21 different product type
    They are : ['Floral' 'Woody' 'Oriental' 'Aromatic' 'Fruity' 'Floral Oriental' 'Oud'
     'Soft Oriental' 'Woody Oriental' 'Citrus' 'Sweet' 'Aquatic' 'Soft Floral'
     'Leather' nan 'Arabian' 'Chypre' 'Floral ' 'Woody,Woody' 'Floral,Fruity'
     'Floral,Woody' 'Aromatic,Oriental']


As above there is families that is repeated twice such as "Woody,Woody" that represent Woody family, so I will change it.
That also will reduce the fragrance family


```python
final_df.Fragrance_Family.replace("Woody,Woody", "Woody", inplace=True)
```


```python
print(f'This dataset consist of : {final_df.Fragrance_Family.nunique()} different product type')
print(f'They are : {final_df.Fragrance_Family.unique()}')
```

    This dataset consist of : 20 different product type
    They are : ['Floral' 'Woody' 'Oriental' 'Aromatic' 'Fruity' 'Floral Oriental' 'Oud'
     'Soft Oriental' 'Woody Oriental' 'Citrus' 'Sweet' 'Aquatic' 'Soft Floral'
     'Leather' nan 'Arabian' 'Chypre' 'Floral ' 'Floral,Fruity' 'Floral,Woody'
     'Aromatic,Oriental']



```python
final_df.Fragrance_Family.value_counts()
```




    Floral               1246
    Oriental             678 
    Woody                594 
    Aromatic             402 
    Fruity               332 
    Citrus               292 
    Floral Oriental      196 
    Woody Oriental       133 
    Leather              89  
    Oud                  84  
    Soft Floral          55  
    Aquatic              51  
    Soft Oriental        28  
    Arabian              22  
    Chypre               7   
    Sweet                3   
    Floral               2   
    Floral,Fruity        2   
    Floral,Woody         2   
    Aromatic,Oriental    1   
    Name: Fragrance_Family, dtype: int64




```python
# Is there a relation between fragrance family and price? top five most expensive fragrance family
top_family = final_df.groupby("Fragrance_Family").agg({'Price' :"mean"}).sort_values(by = "Price", ascending=False)
top_family.head()
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
      <th>Price</th>
    </tr>
    <tr>
      <th>Fragrance_Family</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Chypre</th>
      <td>881.428571</td>
    </tr>
    <tr>
      <th>Woody Oriental</th>
      <td>719.368421</td>
    </tr>
    <tr>
      <th>Leather</th>
      <td>686.741573</td>
    </tr>
    <tr>
      <th>Floral</th>
      <td>680.000000</td>
    </tr>
    <tr>
      <th>Woody</th>
      <td>649.311448</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
import seaborn as sns 
fig = plt.figure(figsize = (18, 8))

# creating the bar plot
sns.histplot(final_df,x="Fragrance_Family",  hue ='Fragrance_Family')
 
plt.xlabel("Fragrance Family")
plt.ylabel("No. of Perfumes")
plt.title("Different Fragrance Family")
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_66_0.png)
    


Floral family is the highest 

#### How many Character in the dataset?


```python
print(f'This dataset consist of : {final_df.Character_x.nunique()} different product type')
print(f'They are : {final_df.Character_x.unique()}')

```

    This dataset consist of : 15 different product type
    They are : ['Romantic' 'Sophisticated' 'Feminine' 'Charismatic' 'Masculine'
     'Extravagant' 'Dynamic' 'Fresh' 'Glamorous' 'Classical' 'Natural'
     'Sensual' 'Modern' 'Woody' 'natural']


Fresh is not a character, so i will replace it with Natural which is more approriate, as well as natural with lowercase "N" 


```python
final_df.Character_x.replace("Fresh", "Natural", inplace=True)
final_df.Character_x.replace("natural", "Natural", inplace=True)
final_df.Character_x.replace("Woody", "Natural", inplace=True)
```


```python
final_df.Character_x.value_counts()
```




    Romantic         2086
    Charismatic      513 
    Natural          314 
    Extravagant      279 
    Feminine         276 
    Dynamic          186 
    Sensual          171 
    Sophisticated    133 
    Glamorous        110 
    Classical        92  
    Masculine        50  
    Modern           29  
    Name: Character_x, dtype: int64




```python
fig = plt.figure(figsize = (18, 8))

# creating the bar plot
sns.histplot(final_df,x="Character_x",  hue ='Character_x')
 
plt.xlabel("Character")
plt.ylabel("No. of Perfumes")
plt.title("Different Characters")
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_73_0.png)
    


#### How ingredents are presented in the dataset?


```python
final_df.Ingredients.head(5)
```




    0    Watermerlon, Kiwi, Pink Cyclamen, Musk, Pink Pepper, Jasmine, Sandalwood, Lemon Tree                                                                                                        
    1    Citrus, mandarin, bergamot, jasmine, pine, cypress, laurel.                                                                                                                                 
    2    Mandarin Orange, lavendar, black currant, petitgrain, jasmine, orange blossom, vanilla, cedar, musk, ambergris                                                                              
    3    Saffron, Cinnamon, Incense, Nutmeg, White Peach, Green Apple & Nepalese Oud, Leaves of Patchouli, delicate Jasmine, Precious Tobacco, Amber, Woody Notes, Vetiver, Vanilla Pods, White Musk.
    4    Spices, Violet, Lavender, Sweet Toffee, Caramel, Cinnamon, Suede, Vanilla, Amber                                                                                                            
    Name: Ingredients, dtype: object



As shown above Ingredents are listed with a comma in between words. Somthing to notice here is the Capitalization some words are in camelcase some in lowercase. Thus, I will transfer all to a lowercase so I don't run into this issue during the recommendations. 


```python
# convert to a lowercase
final_df['Ingredients'] = final_df["Ingredients"].str.lower()
```


```python
final_df.Ingredients.head()
```




    0    watermerlon, kiwi, pink cyclamen, musk, pink pepper, jasmine, sandalwood, lemon tree                                                                                                        
    1    citrus, mandarin, bergamot, jasmine, pine, cypress, laurel.                                                                                                                                 
    2    mandarin orange, lavendar, black currant, petitgrain, jasmine, orange blossom, vanilla, cedar, musk, ambergris                                                                              
    3    saffron, cinnamon, incense, nutmeg, white peach, green apple & nepalese oud, leaves of patchouli, delicate jasmine, precious tobacco, amber, woody notes, vetiver, vanilla pods, white musk.
    4    spices, violet, lavender, sweet toffee, caramel, cinnamon, suede, vanilla, amber                                                                                                            
    Name: Ingredients, dtype: object




```python
# converting Top_note, Middle_note and Base_note to lowercase 
final_df['Top_note'] = final_df["Top_note"].str.lower()
final_df['Middle_note'] = final_df["Middle_note"].str.lower()
final_df['Base_note'] = final_df["Base_note"].str.lower()
```

##### What are the Concentrations in the dataset?


```python
print(f'{final_df.Concentration.nunique()} concentration levels in the dataset')
print(f'{final_df.Concentration.value_counts()}')
```

    11 concentration levels in the dataset
    Eau de Parfum            3085
    Eau de Toilette          829 
    Eau de Cologne           109 
    Perfume Oil              70  
    Extrait de Parfum        62  
    Unknown                  49  
    Parfum                   13  
    Eau de Parfum Intense    3   
    Eau de Parfum            2   
    Eau de Soin              2   
    Eau Fraiche              1   
    Name: Concentration, dtype: int64


#### Is there a relationship between Concentration and perfume price ?


```python
fig = plt.figure(figsize = (18, 8))

# creating the bar plot
sns.stripplot(y="Concentration", x="Price",
             hue="Concentration",
             data=final_df)
 
plt.xlabel("Price")
plt.ylabel("Concentrations")
plt.title("Concentration and Perfume Price")
plt.xticks(rotation=90)
plt.show()
```


    
![png](output_83_0.png)
    


From the plot above we can say that Concentration have no relationship with the price, because "Eau de Parfum" and "Parfum" which are the strongests concentration in Perfumes have middle price range with only 3 outliers that could be cause be another factor like the brand of the perfume. 

#### What are the Years are in the dataset, Are the Perfumes new, or old? 


```python
final_df.Year.unique()
```




    array([2009., 2015., 2019., 2017., 2014., 2016., 2012., 2008., 2020.,
           2004., 2018., 2013., 2000., 2006., 2021., 2007., 2003., 2002.,
           2011., 2010., 1995., 1996., 1998., 2005., 1999., 1990., 1913.,
           1988., 1934., 1997., 2001., 1994., 1991., 1993., 1984., 1981.,
           1983., 1986., 1987., 1966., 1992., 1792., 1989., 1971., 1930.,
           1969., 1920., 1962., 1978., 1976., 1985., 1974., 1955.,   nan,
           1959., 1925., 1980., 1927., 1973., 1972., 1890.])



The dataset contains Perfumes from 1890-2021 which is Great 

#### Handling Missing Values 


```python
final_df.isnull().sum()
```




    Name                0 
    Price               0 
    Description         0 
    Rate                0 
    Rating_count        0 
    image               0 
    Brand               0 
    Gender              0 
    Product_Type        0 
    Character_x         0 
    Fragrance_Family    20
    Size                1 
    Year                21
    Ingredients         0 
    Concentration       14
    Top_note            0 
    Middle_note         6 
    Base_note           15
    dtype: int64



- Filling missing values:
1. Fragrance Family: This column is based on the perfume it self, I will replace NaNs based on the top ingreadents that usually classifies perfumes to different families. 
Resource: https://www.scentstore.com/about/perfume-explained/#:~:text=The%20fragrance%20concentration%20of%20a,of%20the%20fragrance%20is%20greater
2. Size: This column could be filled in with mode: 100 ml
3. Year: from the dataset we can noticed that a lot of perfumes are lanuched in 2017, thus I will fill NaNs with 2017
4. Concentration: This beased on the amout of the alcohol and perfume oils, which isn't specified in any other column,  thus I will replace with "Unknown" 
5. Middle_note: This column is based on the perfume itself, also not all perfumes have 3 notes such as simple perumes could be only created by a top note, so can't be fill in using mode or knnimputer, thus I will fill in with "Unknown" 
6. Base_note: This column is based on the perfume itself, thus I will fill in with "Unknown" 


```python
# Unknown 
final_df.Concentration.fillna("Unknown", inplace=True)
final_df.Middle_note.fillna("Unknown", inplace=True)
final_df.Base_note.fillna("Unknown", inplace=True)
```


```python
# mode 
final_df.Year.fillna(final_df['Year'].mode()[0], inplace=True)
final_df.Size.fillna(final_df['Size'].mode()[0], inplace=True)
```


```python
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("musk")), 'Fragrance_Family'] = 'Oriental'  
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("grapefruit")), 'Fragrance_Family'] = 'Citrus'  
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("leather")), 'Fragrance_Family'] = 'Woody'  
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("rose")), 'Fragrance_Family'] = 'Floral'  
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("lavender")), 'Fragrance_Family'] = 'Aromatic'  
```


```python
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("patchouli")), 'Fragrance_Family'] = 'Oriental' 
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("sandelwood")), 'Fragrance_Family'] = 'Oriental'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("perpper")), 'Fragrance_Family'] = 'Floral'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("iris")), 'Fragrance_Family'] = 'Floral'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("vanilla")), 'Fragrance_Family'] = 'Floral'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("jasmine")), 'Fragrance_Family'] = 'Floral'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("lemon")), 'Fragrance_Family'] = 'Citrus'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("orange")), 'Fragrance_Family'] = 'Citrus'

```


```python
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("amber")), 'Fragrance_Family'] = 'Oriental'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("oakmoss")), 'Fragrance_Family'] = 'Woody'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("tobaco")), 'Fragrance_Family'] = 'Woody'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("cedar")), 'Fragrance_Family'] = 'Woody'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Ingredients'].str.contains("flowers")), 'Fragrance_Family'] = 'Floral'
final_df.loc[(final_df['Fragrance_Family'].isnull()) & (final_df['Top_note'].str.contains("flowers")), 'Fragrance_Family'] = 'Floral'
```


```python
#final_df.to_csv("final_df.csv")
```


### Building the recommender

```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
import missingno as msno
from sklearn.preprocessing import LabelEncoder
#from wordcloud import WordCloud
from textblob import TextBlob
#from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
from sklearn.impute import KNNImputer

from numpy.linalg import norm
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.compose import ColumnTransformer

```


```python
df = pd.read_csv("C:/Users/hmq4/OneDrive/Desktop/Data Science- Misk/chrome/Misk_DSI_Capstone_Project/final_df.csv")
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4239 entries, 0 to 4238
    Data columns (total 18 columns):
     #   Column            Non-Null Count  Dtype 
    ---  ------            --------------  ----- 
     0   Name              4239 non-null   object
     1   Price             4239 non-null   int64 
     2   Description       4239 non-null   object
     3   Rate              4239 non-null   object
     4   Rating_count      4239 non-null   object
     5   image             4239 non-null   object
     6   Brand             4239 non-null   object
     7   Gender            4239 non-null   object
     8   Product_Type      4239 non-null   object
     9   Character_x       4239 non-null   object
     10  Fragrance_Family  4239 non-null   object
     11  Size              4239 non-null   object
     12  Year              4239 non-null   int64 
     13  Ingredients       4239 non-null   object
     14  Concentration     4239 non-null   object
     15  Top_note          4239 non-null   object
     16  Middle_note       4239 non-null   object
     17  Base_note         4239 non-null   object
    dtypes: int64(2), object(16)
    memory usage: 596.2+ KB



```python
# Generate a wordcloud for Perfume ingredients 
#import numpy as np 
#from PIL import Image
#mask = np.array(Image.open("../bottle4.png"))

#wordcloud = WordCloud(background_color="#9085AB",mask=mask, colormap='PiYG').generate(','.join(Dataset['Ingredients']))
# create twitter image
#plt.figure(figsize=[8,8])

#plt.imshow(wordcloud, interpolation="bilinear")
#plt.axis("off")
# store to file
#plt.savefig("perfume.png", format="png")
#plt.show()
```

Note I did the wordcloud in another notebook, because the notebook Iam working on runs locally and I could not install wordcloud, so I did it in another notebook and upload it here. The code I used is above.


```python
#from IPython.display import Image
#Image(filename='image_2.png') 
```


```python
df.shape
```




    (4239, 18)




```python
df.head()
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
      <th>Price</th>
      <th>Description</th>
      <th>Rate</th>
      <th>Rating_count</th>
      <th>image</th>
      <th>Brand</th>
      <th>Gender</th>
      <th>Product_Type</th>
      <th>Character_x</th>
      <th>Fragrance_Family</th>
      <th>Size</th>
      <th>Year</th>
      <th>Ingredients</th>
      <th>Concentration</th>
      <th>Top_note</th>
      <th>Middle_note</th>
      <th>Base_note</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Dolce &amp; Gabanna L'imperatrice 3 Pour Femme</td>
      <td>199</td>
      <td>Perfume for the energetic woman who is a hero ...</td>
      <td>5</td>
      <td>6</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Dolce&amp;Gabbana</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Floral</td>
      <td>100 ml</td>
      <td>2009</td>
      <td>watermerlon, kiwi, pink cyclamen, musk, pink p...</td>
      <td>Eau de Toilette</td>
      <td>pink pepper, kiwi, rhubarb</td>
      <td>jasmine, cyclamen, watermelon</td>
      <td>musk, sandalwood, lemon trees.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Roberto Cavalli Paradiso</td>
      <td>169</td>
      <td>Woody floral fragrance, a subtle aroma that ma...</td>
      <td>4.95</td>
      <td>17</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Roberto Cavalli</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Woody</td>
      <td>50 ml</td>
      <td>2015</td>
      <td>citrus, mandarin, bergamot, jasmine, pine, cyp...</td>
      <td>Eau de Parfum</td>
      <td>citruses , mandarin , bergamot</td>
      <td>jasmine</td>
      <td>cypress, parasol pine, pink laurel</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Yves Saint Laurent Libre</td>
      <td>389</td>
      <td>This perfume is a reflection of Freedom, speci...</td>
      <td>5</td>
      <td>3</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Yves Saint Laurent</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Floral</td>
      <td>90 ml</td>
      <td>2019</td>
      <td>mandarin orange, lavendar, black currant, peti...</td>
      <td>Eau de Parfum</td>
      <td>mandarin orange, lavendar, black currant, peti...</td>
      <td>jasmine, orange blossom</td>
      <td>vanilla, cedar, musk, ambergris</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mancera Red Tobacco</td>
      <td>499</td>
      <td>Mancera Red Tobacco is the new oriental, woody...</td>
      <td>4.38</td>
      <td>8</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Mancera</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Oriental</td>
      <td>120 ml</td>
      <td>2017</td>
      <td>saffron, cinnamon, incense, nutmeg, white peac...</td>
      <td>Eau de Parfum</td>
      <td>saffron, cinnamon, incense, nutmeg, white peac...</td>
      <td>patchouli, jasmine</td>
      <td>tobacco, amber, woody notes, vetiver, vanilla,...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Giorgio Armani Emporio Armani Stronger With Yo...</td>
      <td>399</td>
      <td>for every romantic gentle man, This strong fas...</td>
      <td>5</td>
      <td>3</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Giorgio Armani</td>
      <td>Men</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Aromatic</td>
      <td>100 ml</td>
      <td>2019</td>
      <td>spices, violet, lavender, sweet toffee, carame...</td>
      <td>Eau de Parfum</td>
      <td>pink pepper, juniper, violet leaf</td>
      <td>lavender, sage, toffee, cinnamon</td>
      <td>tonka bean, suede, amber, vanilla</td>
    </tr>
  </tbody>
</table>
</div>



### Recomendations using: 
- Description


```python
from sklearn.feature_extraction.text import CountVectorizer
import emoji

# helper function to get the indices
def get_index(Name):
    return df[df.Name == Name].index[0]

def get_title_from_index(index):
    return df[df.index == index]["Name"].values[0]

# fill in any NaNs 
df['Description'].fillna('')

#step1: create recommendation function
def get_recommendations_2(x):
#step2: creat count matrix 
    cv = CountVectorizer()
    count_matrix = cv.fit_transform(df['Description'])

#step3: cosine similarity 
    cosine_sim = cosine_similarity(count_matrix)

#step4: getting the index of the recommended perfume 
    perfume_index = get_index(x)
    similar_perfumes = list(enumerate(cosine_sim[perfume_index]))
    sorted_similar = sorted(similar_perfumes,key=lambda x:x[1],reverse=True)[1:]

    i=0
    print("Top 5 similar Perfumes to "+x+" are:\n")
    for element in sorted_similar:
        print(emoji.emojize(':star:'), get_title_from_index(element[0]), f"  - Similiarity score: ", format(sorted_similar[i][1], ".4f"))
        i=i+1
        if i>=5:
            break
```


```python
get_recommendations_2("Yves Saint Laurent Libre")
```

    Top 5 similar Perfumes to Yves Saint Laurent Libre are:
    
    ⭐ Lootah Immerse EDP   - Similiarity score:  0.5051
    ⭐ Paco Rabanne 1 Million Lucky   - Similiarity score:  0.4959
    ⭐ Abreez Black Gold Hair Mist    - Similiarity score:  0.4929
    ⭐ Tous Les Jours Perfume Day 39   - Similiarity score:  0.4886
    ⭐ One Direction That Moment   - Similiarity score:  0.4883



```python
get_recommendations_2("Dolce & Gabanna L'imperatrice 3 Pour Femme")
```

    Top 5 similar Perfumes to Dolce & Gabanna L'imperatrice 3 Pour Femme are:
    
    ⭐ Together Forever Set   - Similiarity score:  0.7247
    ⭐ Ya Aali Al Hima Set   - Similiarity score:  0.7130
    ⭐ Ebar Set   - Similiarity score:  0.7130
    ⭐ Men Mithlak? Set   - Similiarity score:  0.6048
    ⭐ Sokar Aleyoon Set   - Similiarity score:  0.5524



```python
get_recommendations_2("Tom Ford Noir Extreme")
```

    Top 5 similar Perfumes to Tom Ford Noir Extreme are:
    
    ⭐ Aigner Blue   - Similiarity score:  0.5635
    ⭐ Calvin Klein CK One Collector's Edition EDT   - Similiarity score:  0.5598
    ⭐ Calvin Klein CK One Collector's Edition EDT   - Similiarity score:  0.5598
    ⭐ Kemi Blending Magic Jabir EDP   - Similiarity score:  0.5588
    ⭐ Van Cleef & Arpels Midnight In Paris   - Similiarity score:  0.5585


Recommending perfume using description is not that useful as the example above all the recomendations are sets that have the perfume we specify in the recommendation function, Thus I will try using other features that is more specific to each perfume such as Ingredients 

### Recomendations using: 
- Gender, Fragrance_Family, Ingredients, Top_note, Middle_note, Base_note


```python
from sklearn.feature_extraction.text import CountVectorizer
import emoji

# helper function to get the indices
def get_index(Name):
    return df[df.Name == Name].index[0]

def get_title_from_index(index):
    return df[df.index == index]["Name"].values[0]

def get_price(index):
    return df[df.index == index]["Price"].values[0]


#step1: select features 
features_df = ['Gender','Fragrance_Family','Ingredients','Top_note','Middle_note','Base_note']

for feature in features_df:
    df[feature] = df[feature].fillna('')

#step2: combine features 
def comine_features(row):
    try:
        return row['Gender']+ " "+row['Fragrance_Family'] + " "+row['Ingredients']+" "+row['Ingredients'] + " "+row['Top_note'] + " "+row['Middle_note']+" "+row['Base_note']
    except:
        print("Error:", row)

df['combined_features'] = df.apply(comine_features, axis=1)

#step3: create recommendation function
def get_recommendations(x):
#step4: creat count matrix 
    cv = CountVectorizer()
    count_matrix = cv.fit_transform(df['combined_features'])

#step5: cosine similarity 
    cosine_sim = cosine_similarity(count_matrix)

#step6: getting the index of the recommended perfume 
    perfume_index = get_index(x)
    similar_perfumes = list(enumerate(cosine_sim[perfume_index]))
    sorted_similar = sorted(similar_perfumes,key=lambda x:x[1],reverse=True)[1:]

    i=0
    print("Top 5 similar Perfumes to "+x+" are:\n")
    for element in sorted_similar:
        print(emoji.emojize(':star:'), get_title_from_index(element[0]),f"     -   {get_price(element[0])} SAR    ", f"  - Similiarity score: ", format(sorted_similar[i][1], ".4f"))
        i=i+1
        if i>=5:
            break

```


```python
df.head()
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
      <th>Price</th>
      <th>Description</th>
      <th>Rate</th>
      <th>Rating_count</th>
      <th>image</th>
      <th>Brand</th>
      <th>Gender</th>
      <th>Product_Type</th>
      <th>Character_x</th>
      <th>Fragrance_Family</th>
      <th>Size</th>
      <th>Year</th>
      <th>Ingredients</th>
      <th>Concentration</th>
      <th>Top_note</th>
      <th>Middle_note</th>
      <th>Base_note</th>
      <th>combined_features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Dolce &amp; Gabanna L'imperatrice 3 Pour Femme</td>
      <td>199</td>
      <td>Perfume for the energetic woman who is a hero ...</td>
      <td>5</td>
      <td>6</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Dolce&amp;Gabbana</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Floral</td>
      <td>100 ml</td>
      <td>2009</td>
      <td>watermerlon, kiwi, pink cyclamen, musk, pink p...</td>
      <td>Eau de Toilette</td>
      <td>pink pepper, kiwi, rhubarb</td>
      <td>jasmine, cyclamen, watermelon</td>
      <td>musk, sandalwood, lemon trees.</td>
      <td>Women Floral watermerlon, kiwi, pink cyclamen,...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Roberto Cavalli Paradiso</td>
      <td>169</td>
      <td>Woody floral fragrance, a subtle aroma that ma...</td>
      <td>4.95</td>
      <td>17</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Roberto Cavalli</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Woody</td>
      <td>50 ml</td>
      <td>2015</td>
      <td>citrus, mandarin, bergamot, jasmine, pine, cyp...</td>
      <td>Eau de Parfum</td>
      <td>citruses , mandarin , bergamot</td>
      <td>jasmine</td>
      <td>cypress, parasol pine, pink laurel</td>
      <td>Women Woody citrus, mandarin, bergamot, jasmin...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Yves Saint Laurent Libre</td>
      <td>389</td>
      <td>This perfume is a reflection of Freedom, speci...</td>
      <td>5</td>
      <td>3</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Yves Saint Laurent</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Floral</td>
      <td>90 ml</td>
      <td>2019</td>
      <td>mandarin orange, lavendar, black currant, peti...</td>
      <td>Eau de Parfum</td>
      <td>mandarin orange, lavendar, black currant, peti...</td>
      <td>jasmine, orange blossom</td>
      <td>vanilla, cedar, musk, ambergris</td>
      <td>Women Floral mandarin orange, lavendar, black ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mancera Red Tobacco</td>
      <td>499</td>
      <td>Mancera Red Tobacco is the new oriental, woody...</td>
      <td>4.38</td>
      <td>8</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Mancera</td>
      <td>Women</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Oriental</td>
      <td>120 ml</td>
      <td>2017</td>
      <td>saffron, cinnamon, incense, nutmeg, white peac...</td>
      <td>Eau de Parfum</td>
      <td>saffron, cinnamon, incense, nutmeg, white peac...</td>
      <td>patchouli, jasmine</td>
      <td>tobacco, amber, woody notes, vetiver, vanilla,...</td>
      <td>Women Oriental saffron, cinnamon, incense, nut...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Giorgio Armani Emporio Armani Stronger With Yo...</td>
      <td>399</td>
      <td>for every romantic gentle man, This strong fas...</td>
      <td>5</td>
      <td>3</td>
      <td>https://assets.goldenscent.com/catalog/product...</td>
      <td>Giorgio Armani</td>
      <td>Men</td>
      <td>Perfume</td>
      <td>Romantic</td>
      <td>Aromatic</td>
      <td>100 ml</td>
      <td>2019</td>
      <td>spices, violet, lavender, sweet toffee, carame...</td>
      <td>Eau de Parfum</td>
      <td>pink pepper, juniper, violet leaf</td>
      <td>lavender, sage, toffee, cinnamon</td>
      <td>tonka bean, suede, amber, vanilla</td>
      <td>Men Aromatic spices, violet, lavender, sweet t...</td>
    </tr>
  </tbody>
</table>
</div>




```python
get_recommendations("Yves Saint Laurent Libre")
```

    Top 5 similar Perfumes to Yves Saint Laurent Libre are:
    
    ⭐ Tous Les Jours Perfume Day 400      -   99 SAR       - Similiarity score:  0.8278
    ⭐ David Kasab Imperial EDP      -   499 SAR       - Similiarity score:  0.7711
    ⭐ Korloff Lady Intense      -   223 SAR       - Similiarity score:  0.6856
    ⭐ Roberto Cavalli Florence      -   167 SAR       - Similiarity score:  0.6715
    ⭐ Guerlain Shalimar Souffle      -   415 SAR       - Similiarity score:  0.6119



```python
get_recommendations("Roberto Cavalli Paradiso")
```

    Top 5 similar Perfumes to Roberto Cavalli Paradiso are:
    
    ⭐ Tous Les Jours Perfume Day 354   - Similiarity score:  0.7239
    ⭐ Lootah Rival EDP   - Similiarity score:  0.4913
    ⭐ Roberto Cavalli Paradiso Azzurro   - Similiarity score:  0.4819
    ⭐ Kun Safi Amber Safi Perfume Oil    - Similiarity score:  0.4445
    ⭐ Kun Safi Amber Safi Perfume Oil    - Similiarity score:  0.4445



```python
get_recommendations("Dolce & Gabbana Pour Femme Intense")
```

    Top 5 similar Perfumes to Dolce & Gabbana Pour Femme Intense are:
    
    ⭐ Korloff Lady Intense   - Similiarity score:  0.6143
    ⭐ Montale Day Dreams   - Similiarity score:  0.6030
    ⭐ Dolce & Gabbana Pour Femme   - Similiarity score:  0.5658
    ⭐ Al Dakheel Oud Bushra Cream    - Similiarity score:  0.5562
    ⭐ Giorgio Armani My Way Intense EDP   - Similiarity score:  0.5525


```python
get_recommendations("Giorgio Armani Emporio Armani Stronger With You Intensely")
```

    Top 5 similar Perfumes to Giorgio Armani Emporio Armani Stronger With You Intensely are:
    
    ⭐ Caron Pour Un Homme De Caron Set    - Similiarity score:  0.5022
    ⭐ Glenn Perri Unbelievable EDT   - Similiarity score:  0.4820
    ⭐ Simone Andreoli Sentosa EDP   - Similiarity score:  0.4273
    ⭐ Al Dakheel Oud Amadora EDP   - Similiarity score:  0.4095
    ⭐ Calvin Klein Obsession for Women   - Similiarity score:  0.4085


```python
#price for this 799 SAR 
get_recommendations("Tom Ford Velvet Orchid")
```

    Top 5 similar Perfumes to Tom Ford Velvet Orchid are:
    
    ⭐ Tous Les Jours Perfume Day 337      -   119 SAR       - Similiarity score:  0.7042
    ⭐ Tom Ford Noir Pour Femme      -   537 SAR       - Similiarity score:  0.5854
    ⭐ Tom Ford Noir Extreme      -   587 SAR       - Similiarity score:  0.5593
    ⭐ Viktor & Rolf Flower Bomb      -   492 SAR       - Similiarity score:  0.5065
    ⭐ Giorgio Beverly Hills Gior Gio      -   160 SAR       - Similiarity score:  0.5050



```python
get_recommendations("Yves Saint Laurent Black Opium")
```

    Top 5 similar Perfumes to Yves Saint Laurent Black Opium are:
    
    ⭐ Musk Alemarat Ceaser      -   122 SAR       - Similiarity score:  0.6361
    ⭐ Tous Les Jours Perfume Day 347      -   99 SAR       - Similiarity score:  0.6143
    ⭐ Lalique Lalique Le Parfum      -   189 SAR       - Similiarity score:  0.5904
    ⭐ Majda Bekkali J'ai Fait Un Re've Obscur      -   1024 SAR       - Similiarity score:  0.5795
    ⭐ Van Cleef & Arpels Collection Extraordinaire Bois d'Amande      -   782 SAR       - Similiarity score:  0.5620


### Conclusion
Perfume Recommendation syatem using perfume data scraped from GoldenScent website https://www.goldenscent.com/en/. Firstly, cleaning the dataset and in the phase I faced many issues because data obtained from goldenscent website have a lot of incosistencies and null values that lead to a lot of problems and have to be resolved before starting the building recommender phase. Second phase was exploring the dataset by doing a proper Exploratory Data Analysis. Third phase building the recommender, the approach chosen was content-based similarity recommender based on cosine similarity. This approach was chosen due to the nature of the dataset in hand where there are no enough rating data avaliable in the dataset.

In Conclusion, two recommender was built in order to see which perform better. first recommender was based on perfume description, unfortanatily it did not perform well. the reason is that not all perfume descriptions have a useful information about the perfume not only this some brand have the same description for all their perfumes. second recommender was based on selected features: Gender, Fragrance_Family, Ingredients, Top_note, Middle_note, Base_note. therfore the second recommender perfome better and produce 5 most similar perfumes with high cosine similarity scores.  
