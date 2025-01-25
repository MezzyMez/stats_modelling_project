# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
The goal of this project is to determine relationships between City Bikes stations in Vancouver, British Columbia, Canada (from the City Bikes API) and their relationship with the surrounding business and attractions in the local area (gathered from FourSquare or YELP places API). The goal is to find a relationship between bike station size (number of bikes possible in a station) and the number of businesses/attractions in multiple chosen catgeories from APIs listing locations.

## Process
### Connect to City Bikes API
All data from Vancouver, BC, Canada was collected through the City Bikes API at https://api.citybik.es/v2/ and stored into a Pandas dataframe to create a dataset of locations to use. - To find the city network ID, the entire network list was downloaded, the searched for 'Vancouver'. This resulted in an ID of 'mobibikes'.
- Response was written into a file in /data/city_bikes.json to avoid repeated calls to API
- Data was loaded through file into a list contain all station data
- Data was parsed into seperate variables containing all required information (name, latitude, longitude, slots)
    - data concerning whether a station has e-bikes was considered but all stations in Vancouver had ebikes, so it was excluded and not helpful.
- Pandas dataframe was created with required info and written to station_df.csv

### Connect to FourSquare API
For each station retrieved from City Bikes, we look up related places from the FourSquare places API and append these to the dataframe. Categories for lookup are 'public_art', 'night_club', 'bicycle_store', 'coffee_shop', 'fast_food', 'museum' & 'grocery_store'.
- API call for 7 categories on 260 stations resulted in 1820 API calls and a 10min runtime. This was done (instead of passing all catgeories as 1 argument) to get as much data from a 50 result limit as possible
- Radius was brought down to 500m to try to get more useful results
- Data was appended to the station_df dataframe created by the City Bikes API using apply().
- New DataFrame was written into a file in /data/foursquare_df.csv
- An example of data before summarization can be found at foursquare.json

### Connect to Yelp API
For each station retrieved from City Bikes, we look up related restaurants & grocery stores within a 1000m radius. The API Call for each category resulted in a 3min call. Categories were harder to defined using the Yelp API thus making the number of businesses returned less usable for our purposes.

### Joining & Cleaning data
Once the data from FourSquare was chosen to be used with City Bikes station data, the dataframes had to be merged. Dataframes were combined and basic exploratory data analysis was done through various functions, basic pairplots and correlation histograms. Outliers were removed using Z-scores over a vlue of 3. Also, any place of interest that had a value of exactly 50 was omitted because this was the maxiumum result of the API call and unlikely to indiciditive or a real quantity of places of interest.

### Storing data in SQL Database
A database called `bike_foursquare` was created to permanently store the resulting data frame in a table called `places` in the data directory.

To address outliers, we removed any bike station slots that had a z-score of greater than 3. In addition, any result for places of interest that were exactly 50 were removed because this represented the maximum result possible rather than an actual number of places. 

### Building a Regression model
A linear regresssion model was proposed to estimate the number of bike slots in a station based on the number of surrounding businesses or attractions within a 500m radius. Using exploratory data analysis and viewing various scatter plots comparing bike slots to results were not suggestive of an obvious correlation. No predictive model was built.

Exploratory Data Analysis was suggestive of a correlation between fast food restaurants and other places of interest (POI). A model was created using all other categories of places (public art, night clubs, bicycle stores, coffee shops, museums & grocery stores) to predict the number of fast food restaurants within 500m of a bike station. 

## Results
There are no significant correlations between bike slots in City Bike stations and the number of surround points of interest within a 500m radius. Linear regression models incompatible with data for these variables.

However, there were correlations to be found between different categories of places. When using fast food restaurants as the dependent variable it was concluded that the best fit scenario had independant variables of night clubs and coffee shops. The number of fast food restaurants could be estimated by determining the number of night clubs and coffee shops in the 500m radius of each bike station.

`fast_food_restaurants = 0.51 + (0.75 * night_club) + (0.31 * coffee_shop)`



## Challenges 
- FourSquare & Yelp API is limited to 50 items per query
- FourSquare API call was long (10 min) due to 1800+ calls for all categories of interest.
- Yelp API categories were hard to navigate and isolate.
- No significant correlations for bike stations slots makes it impossible to work on creating a prediction model.


## Future Goals
- Explore Yelp API more and explore some logistic regression models
- Compare bike_slots in stations to population density api: https://opengateway.telefonica.com/en/apis/population-density-data
