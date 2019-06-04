<h1 style="text-align:center">Identifying Ethnic Neighborhoods of New York City by Food Culture</h1>
<h3 style="text-align:center">Capstone Project: The Battle of Neighborhoods</h3>

<h3 style="text-align:center">Yanbo Ye</h3>
<h3 style="text-align:center">2019-05-20</h3>

## 1. Introduction

New York City is a multicultural city most popular in the United State. The population of New York City is very diverse in ethnicity, which includes non-Hispanic white (English, Italian, Irish, Russian, etc.), Hispanics White(Dominican, Puerto Rican, etc.), black(African, Caribbean, etc.), Asian(Chinese, Japanese, Korean, Indian, etc.).

### 1.1 Problem

Most people like to live in a neighborhood with those of their own ethnic group. In this study, we will try to identify the ethinity of each neighborhood in New York City based on their food culture, specifically, the categories of **Food Venues** from location data.

### 1.2 Interest

Identifying the domiant race of each neighborhood would be helpful to those who want to live or do buissiness in New York City.

## 2. Data

### 2.1 Data Source

The neighborhood name and location data of New York City was downloaded from [New York University](https://geo.nyu.edu/catalog/nyu_2451_34572).

The food venue data with name, category and location of each neighborhood was downloaded from Foursquare's [explore api](https://developer.foursquare.com/docs/api/venues/explore).

The relation of food venue categories was downloaded from Foursquare's [categories api](https://developer.foursquare.com/docs/api/venues/categories).

### 2.2 Data Cleaning

The neighborhood data, oringally in GeoJSON format, was transformed into a table with columns of `Neighborhood`, `Borough`, `Latitude`,`Longitude` and 306 entries.

|     | Neighborhood | Borough | Latitude  | Longitude  |
| --- | ------------ | ------- | --------- | ---------- |
| 0   | Wakefield    | Bronx   | 40.894705 | -73.847201 |
| 1   | Co-op City   | Bronx   | 40.874294 | -73.829939 |
| 2   | Eastchester  | Bronx   | 40.887556 | -73.827806 |
| 3   | Fieldston    | Bronx   | 40.895437 | -73.905643 |
| 4   | Riverdale    | Bronx   | 40.890834 | -73.912585 |

The category data was a nested json format, in which each category has categories attribute to represent it's subcategories. We only take subcategories under the food node and flattened the data into a 347 rows table with the category `Id`, `Name`, `ParentId`. This table will be used to clean the food venue data.

|     |            Id            |          Name           |         ParentId         |
| --- | ------------------------ | ----------------------- | ------------------------ |
| 0   | 503288ae91d4c4b30a586d67 | Afghan Restaurant       | None                     |
| 1   | 4bf58dd8d48988d1c8941735 | African Restaurant      | None                     |
| 2   | 4bf58dd8d48988d10a941735 | Ethiopian Restaurant    | 4bf58dd8d48988d1c8941735 |
| 3   | 4bf58dd8d48988d14e941735 | American Restaurant     | None                     |
| 4   | 4bf58dd8d48988d157941735 | New American Restaurant | 4bf58dd8d48988d14e941735 |

The food venue data within 500 meters radius of each neighborhood was downloaded from Foursquare API. `VenueId`, `VenueName`, `VenueLatitude`, `VenueLongitude`, `VenueCategoryId` and `VenueCategoryName` were extracted into a table of 8347 entries. 137 unique categories were found in these data. For those categories with venues less than 100, we rename the category to their parent if exist. `Sushi Restaurant` with 179 count was also merged into it's parent `Japanese Restaurant`. And then only categories with clear cultural or geographical background(such as `Chinese Restaurant`, `Italian Restaurant`, `Japanese Restaurant`, etc. full list as below.) were taken into account.

|        VenueCategoryName        | Count |
| ------------------------------- | ----- |
| Chinese Restaurant              | 559   |
| Italian Restaurant              | 485   |
| Mexican Restaurant              | 375   |
| American Restaurant             | 360   |
| Asian Restaurant                | 233   |
| Latin American Restaurant       | 220   |
| Japanese Restaurant             | 208   |
| Sushi Restaurant                | 179   |
| Spanish Restaurant              | 151   |
| Thai Restaurant                 | 135   |
| Caribbean Restaurant            | 131   |
| Indian Restaurant               | 124   |
| Korean Restaurant               | 120   |
| French Restaurant               | 120   |
| Mediterranean Restaurant        | 89    |
| Greek Restaurant                | 69    |
| Middle Eastern Restaurant       | 65    |
| Eastern European Restaurant     | 26    |
| Hawaiian Restaurant             | 23    |
| Turkish Restaurant              | 22    |
| African Restaurant              | 15    |
| German Restaurant               | 12    |
| Jewish Restaurant               | 10    |
| Russian Restaurant              | 10    |
| Polish Restaurant               | 7     |
| Halal Restaurant                | 7     |
| Irish Pub                       | 7     |
| Australian Restaurant           | 6     |
| Pakistani Restaurant            | 5     |
| Sri Lankan Restaurant           | 4     |
| Afghan Restaurant               | 4     |
| Swiss Restaurant                | 3     |
| English Restaurant              | 3     |
| Austrian Restaurant             | 3     |
| Caucasian Restaurant            | 3     |
| Scandinavian Restaurant         | 3     |
| Belgian Restaurant              | 2     |
| Ukrainian Restaurant            | 2     |
| Modern European Restaurant      | 2     |
| Portuguese Restaurant           | 2     |
| Czech Restaurant                | 1     |

This will reduce the size of our food venue dataset into 3805 entries with 40 categories and 271 neighborhoods.

|     |         VenueId          |                VenueName                | VenueLatitude | VenueLongitude |     VenueCategoryId      |   VenueCategoryName   | Neighborhood | Borough | Latitude  | Longitude  |
| --- | ------------------------ | --------------------------------------- | ------------- | -------------- | ------------------------ | --------------------- | ------------ | ------- | --------- | ---------- |
| 0   | 508af256e4b0578944c87392 | Cooler Runnings Jamaican Restaurant Inc | 40.898276     | -73.850381     | 4bf58dd8d48988d144941735 | Caribbean Restaurant  | Wakefield    | Bronx   | 40.894705 | -73.847201 |
| 9   | 4c9d5f2654c8a1cd2e71834b | Guang Hui Chinese Restaurant            | 40.876603     | -73.829710     | 4bf58dd8d48988d145941735 | Chinese Restaurant    | Co-op City   | Bronx   | 40.874294 | -73.829939 |
| 16  | 515cc20ce4b0deb133b8e89b | Fish & Ting                             | 40.885539     | -73.829151     | 4bf58dd8d48988d144941735 | Caribbean Restaurant  | Eastchester  | Bronx   | 40.887556 | -73.827806 |
| 22  | 4c632fbaeb82d13a3c5007d6 | Golden Krust Caribbean Bakery and Grill | 40.888543     | -73.831278     | 4bf58dd8d48988d144941735 | Caribbean Restaurant  | Eastchester  | Bronx   | 40.887556 | -73.827806 |
| 25  | 4dbf84a24df0f8fd6b88c9b6 | Royal Caribbean Bakery                  | 40.888252     | -73.831457     | 4bf58dd8d48988d144941735 | Caribbean Restaurant  | Eastchester  | Bronx   | 40.887556 | -73.827806 |
