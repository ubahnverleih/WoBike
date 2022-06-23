# Lime

**Base url**: `https://web-production.lime.bike/api/rider`

## Methods

### Login

There are two methods to Login

1:
+ Request an OTP code that is sent to you by SMS
+ Send back the OTP code to get an Auth token

2:
Use Email to login

#### Request sms code

:warning:  Account must exist  :warning:

**Method**: `GET`

**Path**: `/v1/login`

**Parameters**:

| Parameters | Descriptions             | Mandatory |
| ---------- | ------------------------ | :-------: |
| phone      | phone number intl format | X         |


When entering a phone number, make sure to include the `%2B` before the number instead of the `+` (Example: %2B12222222222 is +1 (222) 222-2222)


**Example**

```bash
curl --request GET \
  --url 'https://web-production.lime.bike/api/rider/v1/login?phone=%2B33612345678'
```

#### Send back OTP code

**Method**: `POST`

**Path**: `/v1/login`

**Header**:

| Header       | Value            | Mandatory |
| ------------ | ---------------- | :-------: |
| content-type | application/json | X         |

**Parameters**:

| Parameters | Descriptions             | Mandatory |
| ---------- | ------------------------ | :-------: |
| phone      | phone number intl format | X         |
| login_code | OTP code (length of 6)   | X         |


**Example**

```bash
curl --request POST \
  --cookie-jar - \
  --url 'https://web-production.lime.bike/api/rider/v1/login' \
  --header 'Content-Type: application/json' \
  --data '{"login_code": "123456", "phone": "+33612345678"}'
```

```JSON
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX3Rva2VuIjoiRk9PQkFSRUlSWkhBMiIsImxvZ2luX2NvdW50IjoyfQ.K5nmvUu92hYXQyeYG6O0rqo0ef2mkp7PMdtp9NrgwOE",
    "user": {
        "id": "FOOBAREIRZHA2",
        "type": "users",
        "attributes": {
            "token": "FOOBAREIRZHA2",
            "phone_number": "33612345678",
            "email_address": null,
            "has_verified_email_address": false,
            "name": "Lime Rider",
            "given_name": "Lime",
            "surname": "Rider",
            "default_payment_method": null,
            "referral_code": "REZHETH",
            "num_trips": 0,
            "edu": false,
            "subscription_item_states": [],
            "juicer_profile_status": null,
            "juicer_profile_initial_activated_at": null,
            "balance_cents": 0,
            "pending_balance_cents": 0,
            "currency": "USD"
        }
    }
}
```

Cookie


```
_limebike-web_session	U0pwQlVjcVRwMXZUTWovcHh3U251MERYTGE2dWpMdFdmNW9sL0d4SHBRVGtZd2VXN20yMjhJczhlR21nUkVHczlEREUweGNpYmtEWVFvQXBoQVNuRWdZWkVBajJPaHhDeStuUmttYVdYYWVsRDJEMUZvNE5YNU4xc1FlcjlDMi8xOVNMcDM3M3JhQWd0TDF2OWphMGR3PT0tLTBDYUtNeUJLcXRmNVd4YnorSEhlTWc9PQ%3D%3D--c8889db210d22bb4a96307b74d39c4b64d48777f
```

#### Login with Email

**Method**: `POST`

**Path**: `/v1/login`

**Header**:

| Header       | Value                             | Mandatory |
| ------------ | --------------------------------- | :-------: |
| Content-Type | application/x-www-form-urlencoded | X         |

**Body**:

`email=<YOUR-EMAIL>&name=Lime%20Rider&password=<YOUR-PASSWORD>&platform=iOS`

**Example**

```
curl --location --request POST 'https://web-production.lime.bike/api/rider/v1/login' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data 'email=<YOUR-EMAIL>&name=Lime%20Rider&password=<YOUR-PASSWORD>&platform=iOS'
```

```JSON
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX3Rva2VuIjoiRk9PQkFSRUlSWkhBMiIsImxvZ2luX2NvdW50IjoyfQ.K5nmvUu92hYXQyeYG6O0rqo0ef2mkp7PMdtp9NrgwOE",
    "user": {
        "id": "FOOBAREIRZHA2",
        "type": "users",
        "attributes": {
            "token": "FOOBAREIRZHA2",
            "phone_number": "33612345678",
            "email_address": null,
            "has_verified_email_address": false,
            "name": "Lime Rider",
            "given_name": "Lime",
            "surname": "Rider",
            "default_payment_method": null,
            "referral_code": "REZHETH",
            "num_trips": 0,
            "edu": false,
            "subscription_item_states": [],
            "juicer_profile_status": null,
            "juicer_profile_initial_activated_at": null,
            "balance_cents": 0,
            "pending_balance_cents": 0,
            "currency": "USD"
        }
    }
}
```

Cookie

```
_limebike-web_session	U0pwQlVjcVRwMXZUTWovcHh3U251MERYTGE2dWpMdFdmNW9sL0d4SHBRVGtZd2VXN20yMjhJczhlR21nUkVHczlEREUweGNpYmtEWVFvQXBoQVNuRWdZWkVBajJPaHhDeStuUmttYVdYYWVsRDJEMUZvNE5YNU4xc1FlcjlDMi8xOVNMcDM3M3JhQWd0TDF2OWphMGR3PT0tLTBDYUtNeUJLcXRmNVd4YnorSEhlTWc9PQ%3D%3D--c8889db210d22bb4a96307b74d39c4b64d48777f
```

### Get Vehicles and Zones

:warning: Auth (bearer AND cookie) are mandatory for this endpoint

**Method**: `GET`

**Path**: `/v1/views/map`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

**Cookie**:

| Cookie                | Mandatory |
| --------------------- | :-------: |
| _limebike-web_session | X         |

**Parameters**:

| Parameters           | Descriptions | Mandatory | Remarks                                                             |
| -------------------- | ------------ | :-------: |---------------------------------------------------------------------|
| ne_lat               | Latitude     | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| ne_lng               | Longitude    | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| sw_lat               | Latitude     | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| sw_lng               | Longitude    | X         | Bounding box for map; apparently ignored in favor of zoom parameter |
| user_latitude        | Latitude     | X         |                                                                     |
| user_longitude       | Longitude    | X         |                                                                     |
| zoom                 | Integer      | X         | When < 15, bikes are clustered                                      |

**Example**

```bash
curl --request GET \
  --url 'https://web-production.lime.bike/api/rider/v1/views/map?ne_lat=52.6&ne_lng=13.5&sw_lat=52.4&sw_lng=13.3&user_latitude=52.5311&user_longitude=13.3849&zoom=16' \
  --header 'authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c' \
  --cookie '_limebike-web_session=N0xLYmE5ZytSSkRZa0FwQUdvYk1TalBaVWwzcnRDWUloT1Y1Z2ZNOVZSc0NCd3ZRZTFOVkxaS2lOcHFpemx6Y1pxT3ZudU1Zenk2ODlYRHBFZ1dxRWtaZGQybzRTQm96V09TWVdycENLcUltMHYzRWlUaEZlMDBqOCt4ODJqSWZwR09PSEtuNDdINnF3VGpkR3g2SjRBPT0tLTlJVHhSVFRDOE1CNm14S203VGxRd2c9PQ%253D%253D--9f55d56be64fefc5d5af3daf9e2fe9f7d7408cc0'
```

```
{
    "data": {
        "id": "views::mapview",
        "type": "map_view",
        "attributes": {
            "regions": null,
            "zones": [
                {
                    "id": "LPAIXU6PU2GMR",
                    "type": "zones",
                    "attributes": {
                        "name": "[Christchurch][SZ][CHCH_SERVICED_AREA]",
                        "polyline": "nnvhG}b_|_@rV|iBdd@pOf_@gr@r}A}bC~F|L|GqIhB_BZaC~CyDRgEdCc@lQs\\xKjMht@euAxJqQ``@ku@VcD{@yi@kk@cqE_`AqdAcTm\\kPat@lFsW~FmgAqr@yh@~Fct@yCiqApt@ucAtA_t@a@mU~FmQ|JcLjLuJnNwGvWoBwD}OuHqJob@wIse@c@_bA_WbSgcBld@m\\zRkb@trA{qAwwAwbDgoBvaDinCvkAumJt}BrFlsGsIr`M_n@tyClo@nbDdfBlsAxtH`sB",
                        "category": "service_zone",
                        "icon_latitude": "-43.52882",
                        "icon_longitude": "172.643155",
                        "show_icon": false,
                        "zone_styling_id": "116"
                    }
                },
                ...
            ],
            "bike_clusters": null,
            "bikes": [
                {
                    "id": "ET-O5RWUHLSWXWTV4FAHYPRUDIJUXXDWKBW4RUDFLQ",
                    "type": "bikes",
                    "attributes": {
                        "status": "locked",
                        "plate_number": "XXX-338",
                        "latitude": -43.551564,
                        "longitude": 172.624372,
                        "battery_percentage": 27,
                        "swappable_battery": false,
                        "last_activity_at": "2021-02-04T04:36:11.000Z",
                        "type_name": "scooter",
                        "battery_level": "low",
                        "meter_range": 1759,
                        "rate_plan": "NZD $1 to unlock +\nNZD $0.38 / 1 min",
                        "bike_icon_id": 48,
                        "last_three": "338",
                        "license_plate_number": null,
                        "brand": "lime",
                        "generation": "2.5"
                    }
                },
		...
            ],
            "selected_bike": null,
            "bike_pins": [],
            "selected_bike_pin": null,
            "parking_spots": null,
            "icons": [
                {
                    "id": "50",
                    "type": "icons",
                    "attributes": {
                        "url": "https://d22d5yy1i19g9i.cloudfront.net/icons/scooter_high_battery.png?fingerprint=a14ab0c75f4838b41729eb1ee746dae9",
                        "description_icon_url": null,
                        "description_link_url": null,
                        "description": "translation missing: en.scooter_high_battery"
                    }
                }
}
```

### Ring Vehicle

If you run this command, it won't work. It will say the vehicle is too far away. Use the `/v1/views/map` path above and set the User Location to where the bike is. You need to also get the ET-ID from a bike, you can acquire it from the same endpoint that I just mentioned. Ring from anywhere!

**Method**: `POST`

**Path**: `/v1/actions/ring_bike`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

**Body**:

`id=<vehicle_ET-ID>`

### View Vehicle info banner

The ET-ID can be found on the 'Get Vehicles and Zones' endpoint.

**Method**: `GET`

**Path**: `/v1/bikes/<ET-ID>/banner`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

**Response**

```
{
    "id": "ET-UM7V7RYMPDPCUYMTKOTCYMEAFPCJ7NN4NZ2T3CA",
    "title": "Available e-bike",
    "header": {
        "image_url": "https://limebike-web-public-assets.s3-us-west-1.amazonaws.com/image_files/JumpBike.png",
        "title": {
            "type": "text",
            "icon": null,
            "value": "Jump IIC262",
            "action": null
        },
        "subtitle": {
            "type": "text",
            "icon": 65,
            "value": "72 km range",
            "action": null
        },
        "action": {
            "icon": 67,
            "type": "ui_flow",
            "value": "expand",
            "text": null,
            "subtext": null
        },
        "expansion": {
            "title": {
                "type": "text",
                "icon": null,
                "value": "Jump IIC262",
                "action": null
            },
            "action": null
        }
    },
    "items": [
        {
            "type": "html",
            "icon": 389,
            "value": "<font color='#000000' size='4'><b>Add payment</b></font><br/><font color='#666666' size='4'>NZD $1 to start, then NZD $0.38/min</font>",
            "action": {
                "icon": 391,
                "type": "deeplink",
                "value": "limebike://payment_methods",
                "text": null,
                "subtext": null
            }
        }
    ],
    "banner_action": {
        "icon": null,
        "type": "ui_flow",
        "value": "reserve",
        "text": "Reserve",
        "subtext": "Free for 10 minutes"
    },
    "icons": [
        {
            "id": 65,
            "url": "https://assets.lime.bike/icons/simplify_unlock/png/ic_battery3_40@3x.png?fingerprint=b3a6d67b61b38995d4d0ef2ff7ae3aca"
        },
        {
            "id": 69,
            "url": "https://assets.lime.bike/icons/simplify_unlock/png/ic_time_40@3x.png?fingerprint=66542356edfa1d5a6b26551a4f2dc1df"
        },
        {
            "id": 389,
            "url": "https://assets.lime.bike/icons/simplify_unlock/png/ic_add_payment_40%403x.png?fingerprint=aba1ad9584d90a6bf7118ea1ed0e7be6"
        },
        {
            "id": 67,
            "url": "https://assets.lime.bike/icons/simplify_unlock/png/ic_carrot_down_40@3x.png?fingerprint=2458978c3bade75743b1c7c768b15f45"
        },
        {
            "id": 296,
            "url": "https://assets.lime.bike/icons/simplify_unlock/png/ic_ring_40@3x.png?fingerprint=3d17ce060e2f3b5dd1afb5038c6ee94c"
        },
        {
            "id": 391,
            "url": "https://assets.lime.bike/icons/simplify_unlock/png/ic_carrot_up_40@3x.png?fingerprint=19b7ee91068c326bda0c8216bf9b8b40"
        }
    ]
}
```

### View your ride history

**Method**: `GET`

**Path**: `/v1/views/ride_history?page_limit=10`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

### GBFS

In select cities, a GBFS feed is also available. Louisville is one for example.

**URL** `https://data.lime.bike/api/partners/v1/gbfs/<city>/gbfs.json`

The Juicer API can be found here: https://github.com/davidwim/lime-juicer/

## Implementations

* https://www.npmjs.com/package/@multicycles/lime (JavaScript)
