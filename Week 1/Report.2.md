# Identifying Ethnic Neighborhoods of New York City by Food Culture

> Capstone Project: The Battle of Neighborhoods

- Yanbo Ye
- 2019-05-20

## Introduction

New York City is a multicultural city most popular in the United State. The population of New York City is very diverse in ethnicity, which includes non-Hispanic white (English, Italian, Irish, Russian, etc.), Hispanics White(Dominican, Puerto Rican, etc.), black(African, Caribbean, etc.), Asian(Chinese, Japanese, Korean, Indian, etc.).

Most people like to live in a neighborhood with those of their own ethnic group. Identifying the domiant race of each neighborhood would be helpful to those who want to live or do buissiness in New York City.

In this study, we will try to identify the ethinity of each neighborhood in New York City based on their food culture, specifically, the categories of **Food Venues** from location data.

## Data

### Data Source

The neighborhood name and location data of New York City was dowloaded from [New York University](https://geo.nyu.edu/catalog/nyu_2451_34572).

The food venue data with name, category and location of each neighborhood was downloaded from Foursquare's [explore api](https://developer.foursquare.com/docs/api/venues/explore).

The relation of food venue categories was downloaded from Foursquare's [categories api](https://developer.foursquare.com/docs/api/venues/categories).

### Data Cleaning

The neighborhood data, oringally in GeoJSON format, was transfromed into a table with columns of `Neighborhood`, `Borough`, `Latitude`,`Longitude` and 306 entries.

The category data was a nested json format, in which each category has categories attribute to represent it's subcategories. We only take subcategories under the food node and flattened the data into a 347 rows table with the category `Id`, `Name`, `ParentId`. This table will be used to clean the food venue data.

The food venue data within 500 meters radius of each neighborhood was downloaded from Foursquare API. `VenueId`, `VenueName`, `VenueLatitude`, `VenueLongitude`, `VenueCategoryId` and `VenueCategoryName` were extracted into a table of 8347 entries. 137 unique categories were found in these data. For those categories with venues less than 100, we rename the category to their parent if exist. This reduce the total category count into 83. And then only categories with clear cultural background (such as Chinese Restaurant, Italian Restaurant, etc.) and count geater than 50 were taken into account, full table as below. This will reduce the size of our food venue data into 3623 entries with 16 categories and 306 neighborhoods.

|     VenueCategoryName     | Count |
| ------------------------- | ----- |
| Chinese Restaurant        | 559   |
| Italian Restaurant        | 485   |
| Japanese Restaurant       | 387   |
| Mexican Restaurant        | 375   |
| American Restaurant       | 360   |
| Asian Restaurant          | 233   |
| Latin American Restaurant | 220   |
| Spanish Restaurant        | 151   |
| Thai Restaurant           | 135   |
| Caribbean Restaurant      | 131   |
| Indian Restaurant         | 124   |
| Korean Restaurant         | 120   |
| French Restaurant         | 120   |
| Mediterranean Restaurant  | 89    |
| Greek Restaurant          | 69    |
| Middle Eastern Restaurant | 65    |
