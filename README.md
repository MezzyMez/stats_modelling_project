# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
The goal of this project is to parse data from bike stations in Vancouver, British Columbia, Canada from the City Bikes API. With the data of all bike locations, we will compare these locations to data available from the FourSquare and Yelp APIs to determine relationships from the bike locations and surrounding attractions or businesses.

## Process
### Connect to City Bikes API
All data from Vancouver, BC, Canada was collected through the City Bikes API at https://api.citybik.es/v2/ and stored into a Pandas dataframe to create a dataset of locations to use. - To find the city network ID, the entire network list was downloaded, the searched for 'Vancouver'. This resulted in an ID of 'mobibikes'.
- Response was written into a file in ../data/city_bikes.json to avoid repeated calls to API
- Data was loaded through file into a list contain all station data
- Data was parsed into seperate variables containing all required information (name, latitude, longitude, slots)
    - data concerning whether a station has e-bikes was considered but all stations in Vancouver had ebikes, so it was excluded and not helpful.
- Pandas dataframe was created with required info

### Connect to FourSquare API
For each station retrieved from City Bikes, we look up related places from the FourSquare places API and append these to the dataframe. Current categories for lookup are ______________


## Results
(fill in what you found about the comparative quality of API coverage in your chosen area and the results of your model.)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
