# Helbiz (Scooter)
[Helbiz Go](https://helbiz.com/go) provides scooter sharing in Italy, Spain and France: https://helbiz.com/cities

The API for getting scooter positions needs full authentication. Therefore, the User Creation Flow is described here:

## Create User

POST `https://api.helbiz.com/prod/user/create`

Headers:
```
X-Requested-With: XMLHttpRequest
User-Agent: Helbiz (com.helbiz.android)
Content-Type: application/json
```

Body:
```
{"email": "helbiz@example.com","password": "...","firstName": "...","lastName": "...","phone": "+12125550100"}
```

Response:
```
{
    "result": {
        "status": "Created",
        "temporaryId": "..."
    }
}
```

You get also sent an SMS with a four digit code. Use this and the `temporaryId` to call the verification endpoint.

## Verify Phone Number

POST `https://api.helbiz.com/prod/user/mobile/verify/{code}/{temporaryId}`

Headers:
```
X-Requested-With: XMLHttpRequest
User-Agent: Helbiz (com.helbiz.android)
Content-Type: application/json
```

Response: HTTP 204

## Login

POST `https://api.helbiz.com/prod/user/authenticate`

Headers:
```
X-Requested-With: XMLHttpRequest
User-Agent: Helbiz (com.helbiz.android)
Content-Type: application/json
```

Body:
```
{"email": "helbiz@example.com","password": "..."}
```

As value for the password field you have to provide the md5 hash of the password sent into the create request.

In order to obtain the hash, supposing you're using a *nix system, you could execute the following command:
```
$ echo -n "password_string" | md5sum | tr --delete '-'
5f4dcc3b5aa765d61d8327deb882cf99
```
then copy and paste the string obtained as result.

Response:
```
{
    "token": "eyJ...kE"
}
```

## Get Regions

GET `https://api.helbiz.com/prod/regions`

Headers:
```
X-Requested-With: XMLHttpRequest
User-Agent: Helbiz (com.helbiz.android)
Content-Type: application/json
X-access-token: eyJ...kE
```


Response:
```
[
  {
    "name": "milano1",
    "center": {
      "lat": 45.465097,
      "lon": 9.188581
    },
    "startTime": "07:00",
    "endTime": "22:00",
    "polygon": [
      [45.47408, 9.16964],
      [45.47591, 9.16811],
      [45.47807, 9.16895],
      [45.47883, 9.17189],
      [45.47771, 9.1753],
      [45.4765, 9.17685],
      [45.47783, 9.18045],
      [45.48445, 9.17753],
      [45.48469, 9.17994],
      [45.48589, 9.18182],
      [45.48812, 9.18246],
      [45.49018, 9.19128],
      [45.49259, 9.19295],
      [45.48951, 9.2012],
      [45.49179, 9.20311],
      [45.48878, 9.21032],
      [45.48637, 9.20809],
      [45.48324, 9.21616],
      [45.47398, 9.20723],
      [45.46097, 9.20792],
      [45.45277, 9.20198],
      [45.45471, 9.21289],
      [45.44773, 9.20774],
      [45.4463, 9.18774],
      [45.45173, 9.18327],
      [45.45274, 9.17643],
      [45.46127, 9.16271],
      [45.47217, 9.16655],
      [45.47408, 9.16964]
    ],
    "bounds": [
      [45.518166, 9.066294],
      [45.519128, 9.369104],
      [45.349039, 9.368418],
      [45.344696, 9.063547]
    ],
    "image": "https://s3-eu-west-1.amazonaws.com/helbiz-avatars/regions/Milan-Icon.png",
    "badge": "https://s3-eu-west-1.amazonaws.com/helbiz-avatars/regions/badges/Milan.png",
    "message": "hours",
    "blacklist": null
  },
  ...
]
```

## Get Scooter Positions

GET `https://api.helbiz.com/prod/scooters?northWest=45.518166,9.066294&southEast=45.349039,9.368418`

Headers:
```
X-Requested-With: XMLHttpRequest
User-Agent: Helbiz (com.helbiz.android)
Content-Type: application/json
X-access-token: eyJ...kE
```

Response:
```
[
    {
        "id": "5bf7d74930212164763155af",
        "lat": 45.46080583333333,
        "lon": 9.202304333333334,
        "batteryLevelInMiles": 8.1,
        "pricing": {
            "hourly": 15
        },
        "geofence": "milano1"
    },
    {
        "id": "5bf7db6030212164763155c3",
        "lat": 45.4560585,
        "lon": 9.195604333333334,
        "batteryLevelInMiles": 15,
        "pricing": {
            "hourly": 15
        },
        "geofence": "milano1"
    },
    ...
]
```

The API returns `[]` when the scooters are not in service, remember to look at the `message` of the region to find out why (eg. hours)
