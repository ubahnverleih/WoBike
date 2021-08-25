# Flamingo
[Flamingo](https://www.flamingoscooters.com/) is an E-Scooter sharing service that operates in New Zealand.

## Request OTP Code

**Method**: `POST`

**URL**: `https://www.googleapis.com/identitytoolkit/v3/relyingparty/sendVerificationCode`

**Headers**:

| Headers                  | Value                                 | Mandatory |
| ------------------------ | ------------------------------------- | :-------: |
| X-Ios-Bundle-Identifier  | com.flamingoscooters.ios              | X         |

**Params**:

| Params        | Value                                  | Mandatory |
| ------------- | -------------------------------------- | :-------: |
| key           | AIzaSyCrkmrEuGrn9tgWfCY2rcpd-LRAnsE84ik| X         |

**The Body**:

`{"iosReceipt":"AEFDNu-Htn7DOuqCRwPrpDA7lMKkUHJduCQPNpel6sVYukJIBM6g_ZoJxyKK__G41ND_-uEkoTQvulRec3wZXXuXxEgfpbcOF8Dftg1eLFHRiZWsCfzbezjYe1P7rx2RIBG7c2V_","iosSecret":"WlQ2n-YexIkrNlpZ","phoneNumber":"+64<YOUR-PHONE-NUMBER>"}`

As a result an OTP code should have been texted to and the response should include the session id:

```
{
    "sessionInfo": "<LONG-STRING>"
}
```

## Verify OTP code

**Method**: `POST`

**URL**: `https://www.googleapis.com/identitytoolkit/v3/relyingparty/verifyPhoneNumber`

**Params**:

| Params        | Value                                   | Mandatory |
| ------------- | --------------------------------------- | :-------: |
| key           | AIzaSyCrkmrEuGrn9tgWfCY2rcpd-LRAnsE84ik | X         |

**Headers**:

| Headers                  | Value                                 | Mandatory |
| ------------------------ | ------------------------------------- | :-------: |
| X-Ios-Bundle-Identifier  | com.flamingoscooters.ios              | X         |

**The Body**:

`{"code":"<OTP-CODE>","operation":"SIGN_UP_OR_IN","sessionInfo":"<SESSION-ID>"}`

This should use the OTP Code and grant you an Access and Refresh Token

```
{
    "idToken": "<ID-TOKEN>", --- Access token
    "refreshToken": "<REFRESH-TOKEN>",
    "expiresIn": "3600",
    "localId": "<LOCAL-ID>",
    "isNewUser": false,
    "phoneNumber": "+64<YOUR-PHONE-NUMBER>"
}
```

### Add User Info

:warning: This only applies to accounts that haven't already been created, but it is recommended that you setup your account in the Flamingo app and not sign up through their API. :warning:

**Method**: `POST`

**URL**: `https://api.flamingoscooters.com/sign-up`

**Headers**:

| Headers                  | Value                                 | Mandatory |
| ------------------------ | ------------------------------------- | :-------: |
| X-Ios-Bundle-Identifier  | com.flamingoscooters.ios              | X         |
| Authorization            | USER-ID                               | X         |

**Params**:

| Params        | Value                                  | Mandatory |
| ------------- | -------------------------------------- | :-------: |
| key           | AIzaSyCrkmrEuGrn9tgWfCY2rcpd-LRAnsE84ik| X         |

**The Body**:

`{"lastName":"<YOUR-LAST-NAME>","email":"<YOUR-EMAIL>","firstName":"<YOUR-FIRST-NAME>","dateOfBirth":"1990-09-20T19:32:00+12:00"}`


## Refresh Token

A token is valid for 1 hour. Use this endpoint to get a new token after the old one expires.

**Method**: `POST`

**URL**: `https://securetoken.googleapis.com/v1/token`

**Headers**:

| Headers                  | Value                                 | Mandatory |
| ------------------------ | ------------------------------------- | :-------: |
| X-Ios-Bundle-Identifier  | com.flamingoscooters.ios              | X         |

**Params**:

| Params        | Value                                   | Mandatory |
| ------------- | --------------------------------------- | :-------: |
| key           | AIzaSyCrkmrEuGrn9tgWfCY2rcpd-LRAnsE84ik | X         |

**The Body**:

`{"grantType":"refresh_token","refreshToken":"<YOUR-REFRESH-TOKEN>"}`

This should verify the code and grant you a Access and Refresh Token. Here's the output:

```
{
    "access_token": "<YOUR-ACCESS-TOKEN>",
    "expires_in": "3600", --- 1 HOUR
    "token_type": "Bearer",
    "refresh_token": "<YOUR-REFRESH-TOKEN>",
    "id_token": "<YOUR-ID-TOKEN>",
    "user_id": "<YOUR-USER-ID>",
    "project_id": "663814096530"
}
```

## Get Scooter Locations

After authenticating you can now retreive locations of scooters and zones

**URL**: `https://production.api.flamingoscooters.com/vehicle/area`

**Method**: `GET`

**Headers**:

| Headers       | Description                           | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| Authorization | Your access token                     | X         |

**Parameters**:

| Parameters   | Descriptions             | Mandatory |
| ------------ | ------------------------ | :-------: |
| neLatitude   | latitude                 | X         |
| neLongitude  | longitude                | X         |
| swLatitude   | latitude                 | X         |
| swLongitude  | longitude                | X         |
| zoom         | zoom integer             | X         |


The output should be something like this:

```
{
    "success": true,
    "data": [
        {
            "id": 23, --- ID to send commands to the scooter
            "object": "vehicle",
            "type": "SCOOTER",
            "model": "SEGWAY_ES",
            "registration": "1038", --- ID to find scooter on the street
            "status": "AVAILABLE",
            "batteryPercent": 0.97,
            "latitude": -43.532892,
            "longitude": 172.633852,
            "statusTime": "2021-02-25T03:50:29.000Z", --- Last Signal with the scooter
            "gpsTime": "2021-02-25T03:41:28.000Z", --- Last GPS Ping
            "remainingRange": 1856, --- KM Range (18km remaining)
            "pricing": {
                "id": 13,
                "object": "pricingPlan",
                "plan": "STANDARD",
                "unlock": 100,
                "perMin": 38,
                "currency": "NZD",
                "description": "$1 NZD to unlock, 38c per min"
            }
        },
        ...
```

## Get Zones and area info

**URL**: `https://production.api.flamingoscooters.com/region/all`

**Method**: `GET`

**Headers**:

| Headers       | Description                           | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| Authorization | Your access token                     | X         |

The output should be something like this:

```
{
    "success": true,
    "data": [
        {
            "id": 4,
            "object": "region",
            "name": "Christchurch",
            "country": "NZ",
            "centreLatitude": -43.532038,
            "centreLongitude": 172.636572,
            "message": null,
            "messageUrl": null,
            "timezone": "Pacific/Auckland",
            "startTime": "00:00",
            "endTime": "24:00",
            "zones": [
                {
                    "id": 90,
                    "object": "zone",
                    "name": "Botanic Gardens",
                    "message": "The Botanic Gardens are a no riding or parking zone.",
                    "type": "NORIDING",
                    "polygon": [
                        {
                            "latitude": -43.529044,
                            "longitude": 172.627315
                        },
                        {
                            "latitude": -43.529519,
                            "longitude": 172.625196
                        },
                        {
                            "latitude": -43.529666,
                            "longitude": 172.623678
                        },
                        {
                            "latitude": -43.529184,
                            "longitude": 172.622128
                        },
                        {
                            "latitude": -43.528274,
                            "longitude": 172.620159
                        },
                        {
                            "latitude": -43.527943,
                            "longitude": 172.620261
                        },
                        {
                            "latitude": -43.527554,
                            "longitude": 172.620068
                        },
                        {
                            "latitude": -43.527329,
                            "longitude": 172.619607
                        },
                        {
                            "latitude": -43.527484,
                            "longitude": 172.618995
                        },
                        {
                            "latitude": -43.52778,
                            "longitude": 172.618539
                        },
                        {
                            "latitude": -43.528609,
                            "longitude": 172.618153
                        },
                        {
                            "latitude": -43.52969,
                            "longitude": 172.617927
                        },
                        {
                            "latitude": -43.530398,
                            "longitude": 172.61819
                        },
                        {
                            "latitude": -43.532035,
                            "longitude": 172.618603
                        },
                        {
                            "latitude": -43.532385,
                            "longitude": 172.619006
                        },
                        {
                            "latitude": -43.532389,
                            "longitude": 172.619725
                        },
                        {
                            "latitude": -43.532233,
                            "longitude": 172.620309
                        },
                        {
                            "latitude": -43.531568,
                            "longitude": 172.623474
                        },
                        {
                            "latitude": -43.531724,
                            "longitude": 172.625084
                        },
                        {
                            "latitude": -43.532179,
                            "longitude": 172.626012
                        },
                        {
                            "latitude": -43.532999,
                            "longitude": 172.62716
                        },
                        {
                            "latitude": -43.533005,
                            "longitude": 172.627323
                        },
                        {
                            "latitude": -43.529044,
                            "longitude": 172.627315
                        }
                    ]
                },
        ...
```

### Get Scooter Parking Photo

Use the `id` from the scooter output and not the `registration` id.

**URL**: `https://api.flamingoscooters.com/vehicle/<vehicle-id>/last-photo`

**Method**: `GET`

**Headers**:

| Headers       | Description                           | Mandatory |
| ------------  | ------------------------------------- | :-------: |
| Authorization | Your access token                     | X         |

This should output the URL for the Parking Photo. Some scooters may not have one, here's an example from a scooter that does have one:

```
{
    "success": true,
    "data": {
        "photoUrl": "https://storage.googleapis.com/flamingo-ugc/JmpviKnyIwwXQQRRmyEJkyEaoasFZpgM.jpg"
    }
}
```

## GBFS

Flamingo also supports the [GBFS](https://github.com/NABSA/gbfs) system.

**URL**: `https://api.flamingoscooters.com/gbfs/<city>/gbfs.json`

The output should be something like this:

```
{
    "success": true,
    "data": {
        "bikes": [
            {
                "bike_id": "4130",
                "lat": -43.528927,
                "lon": 172.639989,
                "current_range_meters": 21420,
                "last_reported": 1629880900
            },
            ...
            ]
    },
    "last_updated": 1629880939,
    "ttl": 60
}
```

**Postman Collection**: https://documenter.getpostman.com/view/11220018/TVKD2xdh

## Implementations

https://openscootermap.netlify.app/

https://moovitapp.com/

https://transitapp.com
