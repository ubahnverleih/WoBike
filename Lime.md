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

### Get bicycles by lat/lng

:warning: Auth (bearer AND cookie) are mandatory for this endpoint

**Method**: `GET`

**Path**: `/v1/views/map`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| authorization | Bearer TOKEN | X         |

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

```JSON
{
    "data": {
        "id": "views::mapview",
        "type": "map_view",
        "attributes": {
            "user": {
                "id": "PSEALP4T4YYZX",
                "type": "users",
                "attributes": {
                    "token": "PSEALP4T4YYZX",
                    "phone_number": "18135196038",
                    "email_address": null,
                    "has_verified_email_address": false,
                    "name": "Lime Rider",
                    "given_name": "Lime",
                    "surname": "Rider",
                    "default_payment_method": null,
                    "referral_code": "R4Y6MQC",
                    "num_trips": 0,
                    "edu": false,
                    "subscription_item_states": [],
                    "promotion_notification": true,
                    "donation_profile": {
                        "id": "F7O24UILJM2E7",
                        "type": "donation_profiles",
                        "attributes": {
                            "donation_type": "none",
                            "donation_organizations": [],
                            "total_donation_amount": {
                                "currency_code": "USD",
                                "amount": 0.0,
                                "amount_str": "0.0",
                                "currency_symbol": "$",
                                "display_string": "$0"
                            },
                            "total_donation_amount_cents": null,
                            "currency": null
                        }
                    },
                    "pod_vehicle_banner_status": null,
                    "juicer_profile_status": null,
                    "juicer_profile_initial_activated_at": null,
                    "accepted_user_agreement_version": 2,
                    "accepted_compliance_version": null,
                    "balance": {
                        "currency_code": "USD",
                        "amount": 0.0,
                        "amount_str": "0.0",
                        "currency_symbol": "$",
                        "display_string": "$0"
                    },
                    "pending_balance": {
                        "currency_code": "USD",
                        "amount": 0.0,
                        "amount_str": "0.0",
                        "currency_symbol": "$",
                        "display_string": "$0"
                    },
                    "new_juicer_earnings_amount": {
                        "currency_code": "USD",
                        "amount": 150.0,
                        "amount_str": "150.0",
                        "currency_symbol": "$",
                        "display_string": "$150"
                    },
                    "new_juicer_earnings_promo_amount": "150",
                    "new_juicer_earnings_promo_amount_cents": 15000,
                    "new_juicer_earnings_promo_amount_cent": 15000,
                    "new_juicer_earnings_promo_currency": "USD",
                    "juicer_referral_type": null,
                    "juicer_referral_code": null,
                    "juicer_referral_amount": null,
                    "juicer_referral_amount_cents": null,
                    "juicer_referral_currency": null,
                    "balance_cents": 0,
                    "pending_balance_cents": 0,
                    "currency": "USD",
                    "show_group_ride": false,
                    "id_verification_pending": false,
                    "current_rental_id": null
                }
            },
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
                },
                
		...
            ],
            "current_level": "city",
            "levels": {
                "region": 7.0,
                "subregion": 11.0,
                "city": 14.0
            },
            "most_recent_trip": null,
            "banner": null,
            "donation_organization": null,
            "parking_spots_radius_meters": 20,
            "map_pins": [],
            "group_ride_id": null,
            "group_ride_participants": null
        }
    },
    "meta": {
        ...
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/lime (JavaScript)
