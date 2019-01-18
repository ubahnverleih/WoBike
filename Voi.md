# Introduction:

VOI (https://voiscooters.com) is a Swedish ride sharing company. They have scooters placed in several cities and countries around the world, including Sweden, Spain, Italy, France and more.

The company has an API that you can access without any authentication needed.

# API overview:

The main endpoint for the VOI API is `https://api.voiapp.io/v1`.

To get a list of available scooters nearby a location, you can use this endpoint:

`https://api.voiapp.io/v1/vehicle/status/ready?la=[LATITUDE HERE]&lo=[LONGTITUDE HERE]`

These are the known accepted request parameters:

`la` - The latitude to perform the search query from.

`lo` - The longtiude to perform the search query from.

