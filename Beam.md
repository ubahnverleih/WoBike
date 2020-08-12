# Beam
[Beam](https://www.ridebeam.com/) is a E-Scooter sharing service that operates in the Asia-Pacific Region.

**Base URL**: `https://gateway.ridebeam.com/api`

## Login

+ Request OTP code
+ Use the OTP code to authenticate

Make sure to change the `latest-app-version` to the latest Beam app version (e.g. `1.38.0`)

After a few updates the API can't pick up any scooters if the `User-Agent` header has an old version in it.

### Request OTP Code

An OTP code is another term for a Verification Code.

**Method**: `POST`

**Path**: `/auth/phoneverification`

**Headers**:

| Headers       | Value                                 | Mandatory |
| ------------- | ------------------------------------- | :-------: |
| Content-Type  | application/json                      | X         |
| User-Agent    | escooterapp/latest-app-version; ios   | X         |

**The Body**:

`{"phoneNumber":"<phone-number>","countryCode":"+<country-code>"}`

As a result, you should end up with info about your Phone Number and the OTP code.

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
     "jwtAccessToken": "JWT <long-string>",
     "refreshToken": "<long-string>",
```

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

```{
                "id": 5019,
                "status": 0,
                "lastReportedBattery": 60,
                "code": "67329",
                "lastOysterHeartbeatReportTime": null,
                "lastPhoneLocationTime": "2020-07-14T03:41:52.773Z",
                "lastReportedTimeTrackerMotion": null,
                "lastOysterAfterMotionSingleReportTime": null,
                "lastPhoneLocationBLERSSI": -100,
                "bestLocation": {
                    "type": "Point",
                    "coordinates": [
                        172.56853366666667,
                        -43.54138616666667
                    ]
                },
                "serialNumber": "N4ZMA2002C0854",
                "vehicleType": 1,
                "bleMacAddress": "ce:c4:6b:35:45:4d",
                "omniIotImei": "868020030569379",
                "Tasks": [],
                "formFactor": "escooter"
            },
```

## Meanings from output

`id`: Some kind of ID used internally - **Shouldn't be included in the app**

`lastReportedBattery`: Battery level of scooter

`code`: The code that lets riders identify the scooter and unlock it

`lastPhoneLocationTime`: Last time that the scooter sent a GPS Ping

`coordinates`: Latitude and Longitude

`serialNumber`: Scooter serial number - **Shouldn't be included in the app**

`bleMacAddress`: Bluetooth address - **Shouldn't be included in the app**

`omniIotImei`: Scooter GPS IMEI - **Shouldn't be included in the app**

`Tasks`: From what I've seen, it says if the scooter is worth an incentive (e.g. Unlock to get $1 credit)

`formFactor`: To tell if it's a bike, escooter, etc
