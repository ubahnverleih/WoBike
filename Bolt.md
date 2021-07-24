
# Bolt (E-Scooters)
[Bolt](https://bolt.eu/) is a E-Scooter sharing service in the EU.

## Get mobile number in international format
This request step is performed by the app after entering your phone number, but seems to be **fully optional**.

Send a ```GET``` request to ```https://user.bolt.eu/user/getMobileNumberInInternationalFormat``` with the following parameters:

| Parameter|Value (examples used in test)|
|-------------|-------------|
|phone|*Phone number (URL encoded!)*|
|version|CI.23.0|
|deviceId|*UUID value*|
|deviceType|iphone|
|device_name|iPhone12,3|
|device_os_version|iOS15.0|
|language|en|

⚠️ The **deviceId** is seemingly used later on for authorization of requests.

## Request SMS OTP
Send a ```POST```request to ```https://user.bolt.eu/user/register/phone``` with the following body:
```json
{
  "phone" : "<phone number, non url-encoded (return value of request above)>",
  "preferred_verification_method" : "sms",
  "phone_uuid" : "UUID Value",
  "appsflyer_id" : "1234567890123-1234567"
}
```

**Parameters are the same as in the request above, but fully optional this time**.

You will receive a response like this:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "verification": {
            "method": "sms",
            "confirmation_data": {}
        },
        "resend_confirmation_interval_ms": 20000
    }
}
```
And an SMS containing the OTP (6 digit numeric).

⚠️ Warning: All requests, even failed ones, will return a 200 Status code. Check ```response.code``` to be 0 for success.

## Submitting the SMS OTP
Send a ```POST``` request to ```https://user.bolt.eu/user/confirmVerification/``` with this body containing the obtained OTP:
``` json
{
  "phone_uuid" : "<UUID>",
  "verification" : {
    "confirmation_data" : {
      "code" : "1234"
    }
  },
  "phone" : "<phone number, non url-encoded>"
}
```

You will receive a response like this: 
```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "id": 12345678,
        "phone": "+4911234680",
        "first_name": "John",
        "last_name": "Doe",
        "email": "your@email.com",
        "birthday": "1980-01-01",
        "allow_sending_news": 1,
        "language": "en",
        "allowed_events": {}
    }
}
```
This seems to mark the used ```deviceId``` as authenticated internally. Reuse it for all future requests.

## Finding nearby scooters
Send a ```GET``` request to ```https://rental-search.bolt.eu/categoriesOverview``` with the following parameters:
|Parameter | Value (examples used in test)|
|--|--|
| lat | 50.00000000 |
| lng | 10.00000000 |
| version | CI.24.0 |
| deviceId | *UUID value* |
| deviceType | iphone |
| device_name | iPhone12,3 |
| device_os_version | iOS15.0 |
| language | de |

The response looks something like this:
```json
{
    "code": 0,
    "message": "OK",
    "data": {
        ...
        "categories": [
            {
                "id": 396,
                "price_rate": {
                    "duration_rate_str": "0.01€/MIN",
                    "unlock_rate_str": "0.00€",
                    "reservation_rate_str": "0.01€/MIN",
                    "full_rate_str": "Entsperren 0.00€ • 0.01€/MIN",
                    "fine_rate_str": "10.00€",
                    ...
                },
                "messages": {
                    "booking": {
                        "confirmation": {
                            "title": "Reservieren für 0.01€/MIN",
                            "description": "Du kannst das Fahrzeug für bis zu 30 Minuten reservieren. Die Abrechnung beginnt sobald du die Reservierung bestätigst."
                        },
                        ...
                    }
                },
                "validations": {
                    "low_battery_warning": 5
                },
                "vehicles": [
                    {
                        "id": 160062,
                        "name": "XXX-798",
                        "type": "scooter",
                        "lat": 49.59605026245117,
                        "lng": 11.002751350402832,
                        "charge": 56,
                        "distance_on_charge": 12960,
                        "search_category_id": 396
                    },
                    ...
                ]
            }
        ],
        "vehicle_type_config": {
            "scooter": {
                "images": {
                    "map_marker_url": "https://images.bolt.eu/mobile/ic_scooter_map_icon.png",
                    "details_url": "https://images.bolt.eu/mobile/scooter_details.png",
                    "parking_photo_overlay_url": "https://images.bolt.eu/mobile/scooter_photo_overlay.png"
                }
            }
        },
        "city_area_filters": [
            {
                "key": "micromobility_vehicle_type",
                "value": "scooter"
            }
        ]
    }
}
```
