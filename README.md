# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
The goal of this project is to parse data from bike stations in Vancouver, British Columbia, Canada from the City Bikes API. With the data of all bike locations, we will compare these locations to data available from the FourSquare and Yelp APIs to determine relationships from the bike locations and surrounding attractions or businesses.

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


## Results
There are no significant correlations between bike slots in City Bike stations and the number of surround points of interest within a 500m radius. Linear regression models incompatible with data for these variables.



## Challenges 
- FourSquare & Yelp API is limited to 50 items per query
- FourSquare API call was long (10 min) due to 1800+ calls for all categories of interest.
- Yelp API categories were hard to navigate and isolate.
- No significant correlations for bike stations slots makes it impossible to work on creating a prediciton model.


## Future Goals
- Explore Yelp API more and explore some logistic regression models
- Compare bike_slots in stations to population density api: https://opengateway.telefonica.com/en/apis/population-density-data
- 
