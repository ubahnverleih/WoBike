

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

All values except the phone number seem to be for analytic purposes only. (uuid too)

You'll receive a response like this:
```json
{
    "code": 0,
    "message": "OK",
    "data": {
	      "phone_number_in_international_format": "+4912345678901"
    }
}
```

## Request SMS OTP
Send a ```POST```request to ```https://user.bolt.eu/user/register/phone``` with the following (json-)body:
```json
{
    "phone" : "<phone number, non url-encoded (return value of request above)>",
    "phone_uuid": "UUID Value"
}
```

| Parameter|Value (examples used in test)|
|-------------|-------------|
|preferred_verification_method|sms|
|version|CI.23.0|
|deviceId|*UUID value*|
|deviceType|iphone|
|device_name|iPhone12,3|
|device_os_version|iOS15.0|
|language|en|

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
And an SMS containing the OTP (4 digit numeric).

⚠️ Warning: All requests, even failed ones, will return a 200 Status code. Check ```response.code``` to be 0 for success.

## Submitting the SMS OTP
Send a ```POST``` request to ```https://node.bolt.eu/user/user/v1/confirmVerification``` with this body containing the obtained OTP:
``` json
{
    "phone" : "<phone number, non url-encoded>",
    "phone_uuid" : "<UUID>",
    "verification" : {
        "confirmation_data" : {
            "code" : "1234"
        }
    }
}
```

Parameters are the following:
| Parameter|Value (examples used in test)|
|-------------|-------------|
|preferred_verification_method|sms|
|version|CI.23.0|
|deviceId|*UUID value*|
|deviceType|iphone|
|device_name|iPhone12,3|
|device_os_version|iOS15.0|
|language|en|

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
This seems to mark the used ```deviceId``` as authenticated internally. Reuse it for all future requests. You must include the header "Authorization" with the value "Basic \<phone number>:\<deviceId>" in Base64.
The ```DeviceId``` parameter seems to be ignored.

## Finding nearby scooters
Send a ```GET``` request to ```https://rental-search.bolt.eu/categoriesOverview``` with the following parameters:
|Parameter | Value (examples used in test)|
|-------------|-------------|
| lat|50.00000000|
|lng|10.00000000|
|version|CI.24.0|
|deviceId|*UUID value*|
|deviceType|iphone|
|device_name|iPhone12,3|
|device_os_version|iOS15.0|
|language|en|

The response looks something like this:
```json
{
    "code": 0,
    "message": "OK",
    "data": {
        ...
        "server_url": "https://germany-rental.taxify.eu",
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

Please note the ```server_url``` aswell as the ```id``` under ```categories``` (city id) as you need them later.

## Get scooter internal information
Gets data about the scooter, including ```vehicle_id```, battery percentage and pricing. 
send a ```GET``` request to ```{server_url}/client/getVehicleDetails```. Replace ```{server_url}``` with the ```server_url``` from prevoius request.
The Endpoint requires Basic authentication.

Parameters:
| Parameter|Value (examples used in test)|
|-------------|-------------|
|vehicle_uuid|*Scooter code under QR*|
|version|CI.23.0|
|deviceId|*UUID value*|
|gps_lat|50.00000000 |
|gps_lng |10.00000000|

Note: ```gps_lat``` and ```gps_lng``` are getting completely ignored.

Response:
```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "search_category_id": 345,
        "validations": {
            "low_battery_warning": 5
        },
        "price_rate": {
            "duration_rate_str": "0.19€/MIN",
            "unlock_rate_str": "0.00€",
            "reservation_rate_str": "0.09€/MIN",
            "full_rate_str": "Unlock 0.00€ • 0.19€/MIN",
            "fine_rate_str": "20.00€"
        },
        "vehicle": {
        "id": 454945,
        "lat": 52.504005,
        "lng": 13.337695,
        "charge": 90,
        "name": "XXX-878",
        "type": "scooter",
        "distance_on_charge": 30400,
        "search_category_id": 345 },
        "city_area_filters": [{
            "key": "search_category_id",
            "value": "345"
        }]
    }
}
```

Here again, is the city id ```value``` under ```city_area_filters```.

## Ring scooter
Gets data about the scooter, including ```vehicle_id```, battery percentage and pricing. 
send a ```GET``` request to ```{server_url}/client/ringVehicle```. Replace ```{server_url}``` with the ```server_url``` from prevoius request.
The Endpoint requires Basic authentication.

Parameters:
| Parameter|Value (examples used in test)|
|-------------|-------------|
|version|CI.23.0|
|deviceId|*UUID value*|
|gps_lat|50.00000000 |
|gps_lng |10.00000000|
|deviceType|iphone|
|device_name|iPhone12,3|
|device_os_version|iOS15.0|
|language|en|

Note: ```gps_lat``` and ```gps_lng``` need to be close to the scooter, but you can simply set it to the exact same location as the scooter. Ring from anywhere!

Body:
```json
{
    "vehicle_id": "123456"
}
```

Vehicle_id can be gotten from "Get Scooter internal information"

Response:
```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "snackbar": {
            "text_html": "<font name=\"body_m\">Scooter XXX-878 is ringing...</font>",
            "id": "ring-vehicle-snackbar"
            }
        }
    }
```

