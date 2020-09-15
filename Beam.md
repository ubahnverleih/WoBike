# Beam
[Beam](https://www.ridebeam.com/) is a E-Scooter sharing service that operates in the Asia-Pacific Region.

**Base URL**: `https://gateway.ridebeam.com/api`

## Login

+ Request OTP code
+ Use the OTP code to authenticate

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

As a result an OTP code should have been texted to and the JSON output should return something like this:

```
{
    "success": true,
    "message": "Text message sent to +XX XX-XXX-XXXX.",
    "response": {
        "carrier": "<Your Carrier",
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

As a result, you should end up with something like this:

```
     {
    "success": true,
    "message": "verify successfully",
    "data": {
        "jwtAccessToken": "JWT <long-string>",
        "refreshToken": "<long-string>",
        "userType": null,
        "userId": <user-ID>,
        "id": <ID>,
        "firstName": null,
        "lastName": null,
        "email": null,
        "phoneNumber": "<your-phone-number",
        "countryCode": "+<country-code>",
        "dateAcceptedConvenienceFeeTerms": null,
        "isNewUser": true,
        "isAgeVerified": null
    }
}
```
Use the `jwtAccessToken` as your Auth Token

## Get Scooter Locations

**Path**: `/vehicles/scooter/latlong`

**Method**: `GET`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |
| Authorization | jwtAccessToken                        | X         |
| Latitude      | latitude                              |           |
| Longitude     | longitude                             |           |

**Parameters**:

| Parameters | Value                    | Mandatory |
| ---------- | ------------------------ | :-------: |
| latitude   | latitude                 | X         |
| longitude  | longitude                | X         |


The output should be something like this:

```
{
    "success": true,
    "message": "success",
    "response": {},
    "error": "",
    "data": {
        "isRideableTime": true,
        "scooters": [
            {
                "id": 4792,
                "status": 0,
                "lastReportedBattery": 71,
                "code": "80863",
                "lastOysterHeartbeatReportTime": null,
                "lastPhoneLocationTime": "2020-09-12T22:46:13.996Z",
                "lastReportedTimeTrackerMotion": null,
                "lastOysterAfterMotionSingleReportTime": null,
                "lastPhoneLocationBLERSSI": -88,
                "bestLocation": {
                    "type": "Point",
                    "coordinates": [
                        172.57569866666665,
                        -43.538005
                    ]
                },
                "serialNumber": "N4ZMA2002C0195",
                "vehicleType": 1,
                "bleMacAddress": "e3:28:17:be:93:dc",
                "omniIotImei": "868020030598071",
                "Tasks": [],
                "formFactor": "escooter"
            },
            
            ...
          ],
        "isPartialResponse": false
    }
}
```

## Meanings from output

`id`: Some kind of ID used internally - **This is NOT the vehicle ID**

`lastReportedBattery`: Battery level of scooter

`code`: The code that lets riders identify the scooter and unlock it

`lastPhoneLocationTime`: Last time the app connected with the scooter (usually last ride)

`coordinates`: Scooter Latitude and Longitude location

`serialNumber`: Scooter serial number

`bleMacAddress`: Bluetooth address

`omniIotImei`: Scooter IMEI

`Tasks`: From what I've seen, it says if the scooter is worth an incentive (e.g. Unlock to get $1 credit)

`formFactor`: To tell if it's a bike, escooter, etc
