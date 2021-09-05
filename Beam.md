# Beam
[Beam](https://www.ridebeam.com/) is a E-Scooter sharing service that operates in the Asia-Pacific Region.

**Base URL**: `https://gateway.ridebeam.com/api`

## Login

+ Request an OTP code to be texted to you
+ Use the OTP code to get an Auth token

Make sure to change the `latest-app-version` to the latest Beam app version (e.g. `1.38.0`). After a few updates the API can't pick up any scooters if the `User-Agent` header has an old version in it. The latest version can be found with the endpoint: `/versions`

### Request OTP Code

**Method**: `POST`

**Path**: `/auth/phoneverification`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------- | ------------------------------------- | :-------: |
| Content-Type  | application/json                      | X         |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |

**The Body**:

`{"phoneNumber":"<phone-number>","countryCode":"+<country-code>"}`

An OTP code should have been texted to you and this should be the response:

```
{
    "success": true,
    "message": "Text message sent to +XX XX-XXX-XXXX.",
    "response": {
        "carrier": "<your-carrier>",
        "is_cellphone": true,
        "message": "Text message sent to +XX XX-XXX-XXXX.",
        "seconds_to_expire": 599,
        "uuid": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
        "success": true
    },
    "error": null,
    "data": {}
}
```

### Send back OTP code

**Method**: `POST`

**Path**: `/auth/verifycodeAndCreateUser`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------- | ------------------------------------- | :-------: |
| Content-Type  | application/json                      | X         |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |

**The Body**:

`{"phoneNumber":"<phone-number>","countryCode":"+<country-code>","code":"<OTP-code>","deviceName":"iPhone8,1(iPhone 6S)"}`

Repsonse:

```
     {
    "success": true,
    "message": "verify successfully",
    "data": {
        "jwtAccessToken": "JWT <access-token>",
        "refreshToken": "<refresh-token>",
        "userType": null,
        "userId": <user-ID>,
        "id": <ID>,
        "firstName": null,
        "lastName": null,
        "email": null,
        "phoneNumber": "<your-phone-number",
        "countryCode": "+<country-code>",
        "dateAcceptedConvenienceFeeTerms": null,
        "isNewUser": false,
        "isAgeVerified": null
    }
}
```
Use the `jwtAccessToken` as your Auth Token

### Refresh Token

**Method**: `POST`

**Path**: `/auth/refreshaccesstoken`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------- | ------------------------------------- | :-------: |
| Content-Type  | application/json                      | X         |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |

**The Body**:

`{"userId":<your-user-id>,"refreshToken":"<your-refresh-token>"}`

Response:

```
     {
    "success": true,
    "response": {
        "accessToken": "<your-new-access-token>"
    }
}
```

## Get Scooter Locations

**Path**: `/vehicles/getForH3/rider/detail`

**Method**: `GET`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |
| Authorization | access-token                          |           |

**Parameters**:

| Parameters | Value                    | Mandatory |
| ---------- | ------------------------ | :-------: |
| latitude   | latitude                 | X         |
| longitude  | longitude                | X         |


Response:

```
{
    "success": true,
    "message": "success",
    "response": {},
    "error": "",
    "data": {
        "vehicles": [
            {
                "code": "81004", --- Vehicle ID written on the scooter
                "formFactor": "escooter",
                "latitude": -41.295956,
                "longitude": 174.778755,
                "batteryLevel": 49,
                "id": 29463, --- Internal ID used for sending commands to the scooter
                "status": 0,
                "lostAndHiddenFromMaps": false,
                "undeployableDamaged": false,
                "vehicleModel": "NINEBOT_MODEL_MAX_SWAPPABLE",
                "lastPhoneLocationTime": "2021-06-04T07:50:30.419Z",
                "lastPhoneLocation": [
                    174.77869774531294,
                    -41.29596706766972
                ],
                "latestOmniIotLocation": [
                    174.778755,
                    -41.295956
                ],
                "latestUserReportedLocation": [
                    174.77597826176768,
                    -41.289671394081324
                ]
            },
            ...
          ],
        "isPartialResponse": false
    }
}
```

## Get Zones

**Path**: `/geoRegion/location`

**Method**: `GET`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |
| Authorization | <access-token>                        |           |

**Parameters**:

| Parameters | Value                    | Mandatory |
| ---------- | ------------------------ | :-------: |
| latitude   | latitude                 | X         |
| longitude  | longitude                | X         |
| userMode   | 0                        | X         |


Response:

```
"response": {
        "polygon": {
            "type": "Polygon",
            "coordinates": [
                [
                    [
                        171.578979492188,
                        -44.0026935032532
                    ],
                    [
                        173.309326171875,
                        -44.0026935032532
                    ],
                    [
                        173.309326171875,
                        -43.0588546064345
                    ],
                    [
                        171.578979492188,
                        -43.0588546064345
                    ],
                    [
                        171.578979492188,
                        -44.0026935032532
                    ]
                ]
            ],
            "crs": {
                "type": "name",
                "properties": {
                    "name": "urn:ogc:def:crs:EPSG::4326"
                }
            }
        },
```

## Find specific scooter info / Last Parking Photo

**Path**: `/vehicles/<internal-ID>/rider`

**Method**: `GET`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |
| Authorization | <access-token>                        |           |

Response:

```
{
    "id": 5124,
    "vehicleType": 1,
    "vehicleModel": "NINEBOT_MODEL_MAX_SWAPPABLE",
    "batteryStateOfChargePercentage": 74,
    "code": "20224",
    "status": 6,
    "estimatedBattery": 74,
    "lastReportedBattery": 74,
    "batteryUpdated": "2020-10-27T21:20:40.138Z",
    "totalLifeTimeRides": 120,
    "lastPhoneLocation": {
        "type": "Point",
        "coordinates": [
            172.6391947,
            -43.5451468
        ]
    },
    "lastPhoneLocationTime": "2020-10-27T06:11:30.750Z",
    "lastOysterAfterMotionSingleReport": null,
    "lastOysterAfterMotionSingleReportTime": null,
    "lastPhoneLocationWasBLEConnected": true,
    "lastPhoneLocationBLERSSI": -67,
    "locationUpdated": "2020-10-27T04:57:26.777Z",
    "lastOysterHeartbeatReport": null,
    "lastOysterHeartbeatReportTime": null,
    "lastReportedLocationTrackerMotion": null,
    "lastReportedTimeTrackerMotion": null,
    "lastCollectedTime": "2020-10-27T04:57:26.777Z",
    "lastRideStartTime": "2020-10-25T04:18:52.170Z",
    "lastActiveTime": "2020-10-25T04:22:06.143Z",
    ...
}
```

## Find Parking Spots

**Path**: `/idealLocation/latlng`

**Method**: `GET`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |
| Authorization | jwtAccessToken                        |           |

**Parameters**:

| Parameters | Value                    | Mandatory |
| ---------- | ------------------------ | :-------: |
| latitude   | latitude                 | X         |
| longitude  | longitude                | X         |


Response:

```
{
        "id": 3436,
        "pinLocation": {
            "type": "Point",
            "coordinates": [
                172.6087478011045,
                -43.49812043502988
            ]
        },
        "currentNumber": 0,
        "idealNumber": 3,
        "radius": 50,
        "deleted": false,
        "createdAt": "2020-08-04T10:21:34.242Z",
        "updatedAt": "2020-08-04T12:14:59.498Z",
        "locationName": "481 Papanui Road, Papanui, Christchurch 8053, New Zealand",
        "message": "A BOOSTER OFFER IS AVAILABLE NEAR HERE",
        "title": "Papanui Road",
        "imageUrl": "https://pin-location-images.s3-ap-southeast-1.amazonaws.com/images/BurgerFuel_Papanui.jpg.jpeg",
        "qrCode": "",
        "isHiddenFromCharger": false,
        "isHiddenFromRider": false,
        "isHiddenFromAdmin": false,
        "subtitle": "470-466 Papanui Road",
        "characteristics": [
            "operation_booster"
        ],
        "properties": {},
        "cityId": 14,
        "geofenceId": 136
    },
```
