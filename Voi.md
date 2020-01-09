# Introduction:

[VOI](https://voiscooters.com) is a Swedish ride sharing company focused on electric scooters. They have scooters placed in several cities and countries around the world, including Sweden, Spain, Italy, France and more.

Base url of the API is `https://api.voiapp.io/v1`


# Authentication:
The API requires an **Access Token**. This token is valid for only a short time (10 minutes I think, I haven't tried to measure). You get the **Access Token** by opening a session. You need an **Authentication Token** to open a session. When opening a session, you get: 
* an **Access Token** that you can use in the next 10 minutes or so to query data
* a new **Authentication Token** to open the next session

To get the first Authentication Token, see steps 1,2,3 below. 

See here example of python implementation of the session authentication (once the step 1-2-3 are done): [https://github.com/hawisizu/scooter_scrapper/blob/master/providers/voi.py](https://github.com/hawisizu/scooter_scrapper/blob/master/providers/voi.py)

## 1 - Get OTP
`POST` request to `https://api.voiapp.io/v1/auth/verify/phone`

Body: raw/JSON
```json
{
    "country_code": "DE",
    "phone_number": "176xxxxxxxx"
}
```
Note: phone number is without any 0. 

In the body of the answer, you get a UUID in a param `token`, and of course an SMS message to the provided phone number. 
```json
{
"token": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
## 2 - Verify OTP
This request gets nothing in the answer, but is necessary so that the token works. The `code` should be the value received by SMS. 

`POST` request to `https://api.voiapp.io/v1/auth/verify/code`

Body: raw/JSON
```json
{
    "code": "123456",
    "token": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
Answer is 204 with no content.

## 3 - Get first authToken
`POST` request to `https://api.voiapp.io/v1/auth/verify/presence`

Body: raw/JSON:
```json
{
    "email": "xxx@xxx.com",
    "token": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```
(the email should apparently be valid)
Answer will be: 
```json
{
    "authenticationToken": "xxxxxxxxxxxxxxxx"
}
```
(the token is really super long)

## 4 - Open Session
`POST` request to `https://api.voiapp.io/v1/auth/session`

Body: raw/JSON
```json
{
    "authenticationToken": "xxxxxxxxxxxxxxxx"
}
```
Answer will be: 
```json
{
    "accessToken": "yyyyyyyyyyyyyyyyy",
    "authenticationToken": "zzzzzzzzzzzzzzzzzzz"
}
```
You will then use the `accessToken` to query the data, and the `authenticationToken` to open the next session, so that you don't have to re-do step 1-2-3

# Access scooter data
Once you have an Access Token, you can query the zones info, or if you already know which zone you want to query, get the scooters of the zone. 

## Get zones info
`GET` request to `https://api.voiapp.io/v1/zones`
In the headers you need the access token you got when opening a session: 
`x-access-token`: `yyyyyyyyyyyyyyyyy`

Answer: a long json with all the zones where VOI is currently operating, with details on the prices and forbidden zones, the max speed, etc. 
See here an example of the full zones json retrieved on Dec 20th, 2019: 
[https://gist.github.com/hawisizu/4d54000dc4d5d6d2e39f6994006b74d2](https://gist.github.com/hawisizu/4d54000dc4d5d6d2e39f6994006b74d2)

Notes: 
* you can also filter the results by passing `lng` and `lat` as params of the GET request.
* in the json there are some test cities

## Get Scooter info 
`GET` request to `https://api.voiapp.io/v1/vehicles/zone/<ZONE_ID>/ready`

`<ZONE_ID>` has to be the ID of one of the zones. 

In the headers you need the access token you got when opening a session: 
`x-access-token`: `yyyyyyyyyyyyyyyyy`

Answer: 
```json
[
  {
    "id":"b12a8bcd-d024-4b70-9565-b5d295fe93c0",
    "short":"anbc",
    "name":"VOI",
    "zone":2,
    "type":"btv1",
    "status":"ready",
    "bounty":0,
    "location":[48.858737,2.33287],
    "battery":100,
    "locked":true,
    "updated":"2019-01-19T05:50:33.319335Z"
  },
  {
    "id":"d0ddebea-7bd9-47f0-846a-b33184fdd158",
    "short":"c96a",
    "name":"VOI",
    "zone":9,
    "type":"btv1",
    "status":"ready",
    "bounty":0,
    "location":[48.862964444444444,2.304696111111111],
    "battery":93,
    "locked":true,
    "updated":"2019-01-19T15:00:23.908909Z"
  },
  ...
]
```

The response is a list with information about each scooter, in a JSON format. Here is a list of the parameters in each scooter object and information about what they (most likely) do:

`id`: A scooter ID that most likely is used internally.

`short`: A four-character shortcode of the VOI scooter, which is used to unlock it in the app if you don´t scan the QR code on the scooter.

`name`: The scooter name, which seems to be "VOI" in most cases.

`zone`: This is most likely an indication of what zone the scooter is located in. There is no known zone ID list right now. Each VOI zone seems to be represented by an integer.

`type`: Most likely the model of the VOI scooter. So far, I haven´t gotten another value of "btv1" as the response. I´m not sure if VOI is using multiple models of scooters or not, or if they have only one specific model of electric scooters in one city. That might be an explaination to why the value often is the same.

`status`: Indicates the scooter status. As the request example above is for scooters that are ready, this value should be "ready".

`bounty`: I have no idea what this value means, and it seems to be 0 in most cases.

`location`: A list of the scooter location in lat/lon (latitude/longitude) format.

`battery`: An integer representing the battery percentage of the scooter.

`locked`: Indicates if the scooter is locked or not. I don´t know what the difference between these two states are exactly though.

`updated`: A time object that most likely refers to when the scooter was latest updated. I don´t know if it refers to the time when the scooter was most recently used or when the time when the scooter most recently connected/uploaded data to the VOI severs.
