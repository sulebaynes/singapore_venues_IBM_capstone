# Exploring Venues in Singapore - Hands-on Project making use of Foursquare API with Python

This report serves as a medium to make you get acquainted with my capstone project - the final leg  of the IBM Data Science Professional Certificate on Coursera online learning platform. It focuses on Singapore districts and intends to give hints to those who are searching for a neighbourhood in Singapore to move which is not only affordable, also a popular area with cafes and bars as well as it offers other various activities. The data explored is used to cluster the districts based on the venue category. Moreover the rental price (Singaporean dollar /studio apartment) and venue density (Number of venues/meter district) are also analysed. 
This project targets singles rather than families as the rental prices focus on studio apartments.

## 1. Introduction
The Regions of Singapore are urban planning subdivisions demarcated by the Urban Redevelopment Authority of Singapore to aid in its planning efforts. Over time, other governmental organisations have also adopted the five regions in their administrative work, as for example the Department of Statistics in the census of 2000. The regions are further subdivided into 55 planning areas, which include two water-catchment areas. The largest region in terms of area is the West Region with 201.3 km2 (77.7 sq mi), while the Central Region is the most populous with an estimated population of 922,980 inhabitants in the area in 2019.[1]
Planning Areas, also known as DGP areas or DGP zones, are the main urban planning and census divisions of Singapore delineated by the Urban Redevelopment Authority. There are a total of 55 of these areas, organised into five regions (Central Region, East Region, North Region, North-East Region and West Region). A Development Guide Plan is then drawn up for each planning area, providing for detailed planning guidelines for every individual plot of land throughout the country[2].
The Central Area, also called the City Area, and informally The City, is the city centre of Singapore. Located in the south-eastern part of the Central Region, the Central Area consists of eleven constituent planning areas, the Downtown Core, Marina East, Marina South, the Museum Planning Area, Newton, Orchard, Outram, River Valley, Rochor, the Singapore River and Straits View, as defined by the Urban Redevelopment Authority. The term Central Business District (CBD) has also been used to describe most of the Central Area as well, although its boundaries lie within the Downtown Core [3].

## 2. Methodology
### 1. Data Preparation

Acquiring the latitude and longitude coordinates for each district:
The individual coordinates are not available for each of 28 districts as possibly they are made of multiple areas at a time. To tackle this problem we will collect these coordinates for each area then average them by arithmetic mean per district. For instance: District 1 covers the locations Raffles Place, Cecil, Marina, People's Park as given in Table 1. The coordinates of these four locations will be averaged to determine the final coordinates of District 1. 
Acquiring the pricing data:
The price data is pulled from www.squarefoot.com.sg for places with a size of 500 to 600 square foot. The reason for this square foot range is that it is possible to find price data for more districts (only 2 districts are missing). This range (corresponding to 46 to 57 sqm) is sufficient for a single person to live in and does not target families as mentioned previously.
### 2. Venue collection
The coordinate data of each district are supplied to Foursquare API and a limited number of venues in a given radius are pulled from the database. 
### 3. District Clustering
The districts will be clustered according to most common venue types to give the reader more insights for a certain district. 
### 4. Visualisation & discussion
The data is visualised & the results are discussed.

## 2.1. Data Preparation
The districts data is scraped from the Wikipedia website with the use of the python package 'BeautifulSoup' converting it to json format. 
The data is then converted to a data frame using pandas library. The 'General location' data is split into individual locations to make each location readily available for the coordinates collection. 
Using positionstack API the latitude and longitude data are collected for each location looping through the data frame. Via for loop also the latitude and longitude values are averaged based on the number of locations per district. 
The resulting data frame is used for the next step - which is the collection of venue data using Foursquare API. 
The 2020 price data on www.squarefoot.com.sg exists as 9 different HTML tables. As it would be quite redundant to collect only 26 rental prices by writing code, this is done manually. A data frame is created (notice that the data on Districts 24 and 26 are missing).
## 2.2. Venue Collection
The venue data is collected supplying the Foursquare API with the coordinates obtained in the previous step. To be able to use Foursquare API one needs to get a private client id and client secret and to set the limit of the venues and the radius arbitrarily. After defining those requisite variables all the venues for each coordinate are returned within a given radius (Here the radius is set not fewer than 1200 meters to allow gathering as many venues as possible for more rural districts making the clustering possible later. Besides this is a very realistic distance to be explored as it is quite easily walk-able. Number of venues is limited by 100. 
The venues then are grouped based on districts.
As it would be hard to cluster the districts with fewer venues some districts are omitted. 
The data frame containing the data on venue category is prepared for clustering by first one hot encoding later normalising the number of venues in each category per district.
## 2.3. District Clustering 
The elbow method is used to determine the optimum number of clusters before applying any kind of clustering algorithm. The optimum number of clusters are defined.
K-means clustering is one of the simplest and popular unsupervised machine learning algorithms. This algorithm is applied to the weighted data on grouped categories. These clusters are displayed on Singapore map using Folium python library.
Most common 10 venue types are listed for each district belonging to different clusters.
## 2.4. Visualisation
A graph is plotted to serve as an indication of popularity of a district (number of venues) versus its rental prices. Therefore the price data is binned into three categories as low , medium and high. The price of a studio apartment to rent in each district is then designated into these bins (called as 'bucket' below) and the colours in traffic lights are assigned for each bucket (low price = green, medium price = yellow, high price = red). A second graph plots the rental prices of each district based on the cluster they belong to.

## 3. Discussion
The districts of Singapore in terms of venue types, frequencies and rental prices data are explored. By looking at the results of this project one could get an idea about what kind of neighbourhood he could live in with a given budget to spend on an apartment. The popularity of a district is assumed to be measurable by the numbers of venues around that particular district. Districts are then clustered based on the type of venues they have. 
For instance someone with a budget of 2000 dollars per month to spend on rent could rather prefer some more adventageous districts as there have variety of venues with high frequency as seen from the graphs.
