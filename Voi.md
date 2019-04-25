# Introduction:

[VOI](https://voiscooters.com) is a Swedish ride sharing company focused on electric scooters. They have scooters placed in several cities and countries around the world, including Sweden, Spain, Italy, France and more.

The company has an API that you can access without any authentication needed.

# API overview:

The main endpoint for the VOI API is `https://api.voiapp.io/v1`.

To get a list of available scooters nearby a location, you can use this endpoint:

`https://api.voiapp.io/v1/vehicle/status/ready?la=[LATITUDE HERE]&lo=[LONGTITUDE HERE]`

The request above should be sent as a GET request. As stated earlier, no special authentication or API keys are needed.

These are the known accepted request parameters:

`la` - The latitude to perform the search query from.

`lo` - The longtiude to perform the search query from.

**A request with invalid la/lo parameters will still be accepted. You can even leave out the parameters and still get a list of available scooters.**

There are also other `status` available. Change the `ready` in the url to one of these: `'ready', 'bounty', 'riding', 'home', 'collected', 'lost'`

An example request is: `https://api.voiapp.io/v1/vehicle/status/ready?lat=59.329323&lng=18.068581`.

**The server will return a response formatted like this:**

```[{"id":"b12a8bcd-d024-4b70-9565-b5d295fe93c0","short":"anbc","name":"VOI","zone":2,"type":"btv1","status":"ready","bounty":0,"location":[48.858737,2.33287],"battery":100,"locked":true,"updated":"2019-01-19T05:50:33.319335Z"},{"id":"d0ddebea-7bd9-47f0-846a-b33184fdd158","short":"c96a","name":"VOI","zone":9,"type":"btv1","status":"ready","bounty":0,"location":[48.862964444444444,2.304696111111111],"battery":93,"locked":true,"updated":"2019-01-19T15:00:23.908909Z"},...]```

The response is a list with information about each scooter, in a JSON format. Here is a list of the parameters in each scooter object and information about what they (most likely) do:

`id`: A scooter ID that most likely is used internally.

`short`: A four-character shortcode of the VOI scooter, which is used to unlock it in the app if you don´t scan the QR code on the scooter.

`name`: The scooter name, which seems to be "VOI" in most cases.

`zone`: This is most likely an indication of what zone the scooter is located in. There is no known zone ID list right now. Each VOI zone seems to be represented by an integer.

`type`: Most likely the model of the VOI scooter. So far, I haven´t gotten another value of "btv1" as the response. I´m not sure if VOI is using multiple models of scooters or not, or if they have only one specific model of electric scooters in one city. That might be an explaination to why the value often is the same.

`status`: Indicates the scooter status. As the request example above is for scooters that are ready, this value should be "ready".

`bounty`: I have no idea what this value means, and it seems to be 0 in most cases.

`location`: A list of the scooter location in lat/lon (latitude/longtitude) format.

`battery`: An integer representing the battery percentage of the scooter.

`locked`: Indicates if the scooter is locked or not. I don´t know what the difference between these two states are exactly though.

`updated`: A time object that most likely refers to when the scooter was latest updated. I don´t know if it refers to the time when the scooter was most recently used or when the time when the scooter most recently connected/uploaded data to the VOI severs.

**Small notice about API usage: This is an inoffical API documentation. Please don´t overuse the VOI API or any other unoffical APIs in this list. When I checked the request frequency, I noticed that the offical VOI app does a request to the API about every time that you move the map. Even if that is the case, use the API responsibly. No one of the contributors or owners of this repository will take responsibility for bad actions using this information ;)**

Also look at this [Blogpost](https://medium.com/@h_martos/remote-live-tracking-of-voi-scooters-351689ba3bb9).
