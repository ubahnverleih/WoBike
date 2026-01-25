# Lime

### GBFS

In select cities, a GBFS feed is available. The feed now includes internal vehicle IDs in deeplink URIs.

**URL** `https://data.lime.bike/api/partners/v1/gbfs/<city>/gbfs.json`

**Free Bike Status** `https://data.lime.bike/api/partners/v1/gbfs/<city>/free_bike_status`

The `free_bike_status` endpoint returns vehicle data including internal IDs:

```json
{
  "bike_id": "<uuid>",
  "lat": 37.785,
  "lon": -122.41,
  "is_reserved": false,
  "is_disabled": false,
  "rental_uris": {
    "android": "limebike://map?selected_vehicle_id=<vehicle_id>&generated_at=<timestamp>",
    "ios": "limebike://map?selected_vehicle_id=<vehicle_id>&generated_at=<timestamp>"
  }
}
```

## Cities
| United States    | Germany, Austria and Switzerland | France    | Canada   | Israel    | Norway and Italy | Australia and New Zealand | Belgium   |
|------------------|----------------------------------|-----------|----------|-----------|------------------|---------------------------|-----------|
| atlanta          | hamburg                          | paris     | kelowna  | tel_aviv  | oslo             | auckland                  | antwerp   |
| baltimore        | oberhausen                       | marseille | edmonton |           | rome             | sydney                    | brussels  |
| chicago          | opfikon                          |           |          |           | verona           |                           |           |
| cleveland        | reutlingen                       |           |          |           |                  |                           |           |
| colorado_springs | solingen                         |           |          |           |                  |                           |           |
| denver           | zug                              |           |          |           |                  |                           |           |
| detroit          |                                  |           |          |           |                  |                           |           |
| grand_rapids     |                                  |           |          |           |                  |                           |           |
| louisville       |                                  |           |          |           |                  |                           |           |
| new_york         |                                  |           |          |           |                  |                           |           |
| norfolk_va       |                                  |           |          |           |                  |                           |           |
| oakland          |                                  |           |          |           |                  |                           |           |
| san_francisco    |                                  |           |          |           |                  |                           |           |
| san_jose         |                                  |           |          |           |                  |                           |           |
| seattle          |                                  |           |          |           |                  |                           |           |
| washington_dc    |                                  |           |          |           |                  |                           |           |


---
**Base URL**: `https://web-production.lime.bike/api/rider`

## Request Headers

Modern versions of the app (v3.248+) use the following headers:

| Header         | Value                                              | Mandatory |
| -------------- | -------------------------------------------------- | :-------: |
| Authorization  | Bearer TOKEN                                       | X         |
| Platform       | Android or iOS                                     |           |
| App-Version    | 3.248.1                                            |           |
| Content-Type   | application/json                                   |           |
| X-Device-Token | Random UUID                                        |           |

## Methods to authenticate

### Login with phone number

#### Request sms code

:warning:  Account must exist  :warning:

**Method**: `GET`

**Path**: `/v1/login`

**Parameters**:

| Parameters | Descriptions             |
| ---------- | ------------------------ |
| phone      | phone number intl format |


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

| Header       | Value            |
| ------------ | ---------------- |
| content-type | application/json |

**Parameters**:

| Parameters | Descriptions             |
| ---------- | ------------------------ |
| phone      | phone number intl format |
| login_code | OTP code (length of 6)   |


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

### Login with Email

#### Send Magic Link

**Method**: `POST`

**Path**: `/v2/onboarding/magic-link`

**Body**:

| Parameters                  | Descriptions             |
| ----------                  | ------------------------ |
| email                       | your email               |
| user_agreement_version      | 4                        |
| user_agreement_country_code | US                       |

**Example**

```
curl 'https://web-production.lime.bike/api/rider/v2/onboarding/magic-link' \
-X POST \
-d 'email=<your-email>&user_agreement_country_code=US&user_agreement_version=4'
```

#### Use Magic Link

**Method**: `POST`

**Path**: `/v2/onboarding/login`

**Header**:

| Header         | Value         |
| -------------- | ------------- |
| X-Device-Token | random uuid |

**Body**:

`magic_link_token=<your-magic-link-token`

**Example**

```
curl 'https://web-production.lime.bike/api/rider/v2/onboarding/login' \
-X POST \
-H 'X-Device-Token: 43fd2a25-56c5-4d1d-b6c0-a1dab08d8e1d' \
-d 'magic_link_token=<your-magic-link-token>'
```

## Use API

### Get Vehicles and Zones (v1 - Legacy)

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
  --header 'authorization: Bearer <token>' \
  --cookie '_limebike-web_session=<session>'
```

### Get Bike Pins (v2)

The v1 map endpoint is being replaced by separate v2 endpoints in the mobile app.

**Method**: `GET`

**Path**: `/v2/map/bike_pins`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

**Parameters**:

| Parameters     | Descriptions              | Mandatory |
| -------------- | ------------------------- | :-------: |
| ne_lat         | Northeast latitude        | X         |
| ne_lng         | Northeast longitude       | X         |
| sw_lat         | Southwest latitude        | X         |
| sw_lng         | Southwest longitude       | X         |
| user_latitude  | User's current latitude   | X         |
| user_longitude | User's current longitude  | X         |
| zoom           | Map zoom level (15+ for individual pins) | X |

**Example**

```bash
curl --request GET \
  --url 'https://web-production.lime.bike/api/rider/v2/map/bike_pins?ne_lat=37.79&ne_lng=-122.40&sw_lat=37.78&sw_lng=-122.42&user_latitude=37.785&user_longitude=-122.41&zoom=16.0' \
  --header 'Authorization: Bearer <token>'
```

### Get Static Map Elements (v2)

Returns zones, parking areas, no-ride zones.

**Method**: `GET`

**Path**: `/v2/map/static_elements`

Same parameters as bike_pins.

### Get Map Extra Info (v2)

**Method**: `GET`

**Path**: `/v2/map/extra_info`

**Parameters**:

| Parameters     | Descriptions              | Mandatory |
| -------------- | ------------------------- | :-------: |
| user_latitude  | User's latitude           | X         |
| user_longitude | User's longitude          | X         |

---

### Ring Vehicle

If you run this command, it won't work. It will say the vehicle is too far away. Use the `/v1/views/map` path above and set the User Location to where the bike is. You need to also get the ET-ID from a bike, you can acquire it from the same endpoint that I just mentioned. Ring from anywhere!

**Method**: `POST`

**Path**: `/v1/bikes/<ET-ID>/ring`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

**Body**:

`id=<vehicle_ET-ID>`

If you get a 404 "Resource not found" message, it's most likely because the ET-ID provided is invalid.

### View Vehicle info banner

The ET-ID can be found on the 'Get Vehicles and Zones' endpoint.

**Method**: `GET`

**Path**: `/v1/bikes/<ET-ID>/banner`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

If you get a 404 "Resource not found" message, it's most likely because the ET-ID provided is invalid.

### Get Area Rate Plan

Returns pricing for a specific location.

**Method**: `GET`

**Path**: `/v1/bikes/area_rate_plan`

**Parameters**:

| Parameters | Descriptions              | Mandatory |
| ---------- | ------------------------- | :-------: |
| latitude   | Location latitude         | X         |
| longitude  | Location longitude        | X         |
| accuracy   | GPS accuracy in meters    |           |
| gps_time   | ISO 8601 timestamp        |           |

**Example**

```bash
curl --request GET \
  --url 'https://web-production.lime.bike/api/rider/v1/bikes/area_rate_plan?latitude=37.785&longitude=-122.41&accuracy=5.0' \
  --header 'Authorization: Bearer <token>'
```

---

### View your ride history

**Method**: `GET`

**Path**: `/v1/views/ride_history?page_limit=10`

**Header**:

| Header        | Value        | Mandatory |
| ------------- | ------------ | :-------: |
| Authorization | Bearer TOKEN | X         |

### View Trip Receipt (v2)

**Method**: `GET`

**Path**: `/v2/views/trip_receipt`

**Parameters**:

| Parameters | Descriptions                                      |
| ---------- | ------------------------------------------------- |
| trip_id    | Trip ID (13-char alphanumeric format)             |

### View Trip Summary (v2)

**Method**: `GET`

**Path**: `/v2/views/trip_summary`

**Parameters**:

| Parameters     | Descriptions                          |
| -------------- | ------------------------------------- |
| transaction_id | Base32-encoded transaction ID         |
| source         | Source context (e.g., `transaction_history`) |

### View User Transactions

**Method**: `GET`

**Path**: `/v1/views/user_transactions`

Returns list of past transactions/rides.

---

## User/Account Endpoints

### Bootstrap (App Initialization)

**Method**: `GET`

**Path**: `/v1/users/bootstrap`

**Parameters**:

| Parameters | Descriptions              |
| ---------- | ------------------------- |
| latitude   | Current latitude          |
| longitude  | Current longitude         |
| android_id | Android device ID         |

Returns user configuration, feature flags, and initial app state.

### Side Menu

**Method**: `GET`

**Path**: `/v1/users/side_menu`

Returns menu items and badge counts.

### User Groups

**Method**: `GET`

**Path**: `/v1/user/groups`

Returns user's group memberships (enterprise accounts, etc.).

### Wallets

**Method**: `GET`

**Path**: `/v1/wallets`

Returns payment methods and balances.

### Referral Credits

**Method**: `GET`

**Path**: `/v1/views/referral_credits`

Returns referral program status and credits.

### Rider Summary

**Method**: `GET`

**Path**: `/v1/views/rider_summary`

Returns user statistics and ride summary.

---

## App State/Safety Endpoints

### App State

**Method**: `GET`

**Path**: `/v2/app_state`

Returns current application state and configuration.

### Curfew Check

**Method**: `GET`

**Path**: `/v2/curfew/check`

Returns curfew restrictions for current location (used in cities with ride time limits).

### Rider Map Start Blockers

**Method**: `GET`

**Path**: `/v1/views/rider_map_start_blockers`

**Parameters**:

| Parameters     | Descriptions    |
| -------------- | --------------- |
| user_latitude  | User's latitude |
| user_longitude | User's longitude|

Returns any blockers preventing ride start (payment issues, account status, etc.).

### Vehicle Filter Banner

**Method**: `GET`

**Path**: `/v1/views/vehicle_filter_banner/show`

Returns vehicle type filter configuration.

### Safety Center Menu

**Method**: `GET`

**Path**: `/v1/menus/safety_center`

Returns safety center menu configuration and resources.

---

## Destination Endpoints

### Destination Info Card

**Method**: `GET`

**Path**: `/v1/destinations/info_card`

### Pre-Trip Planner Ingress

**Method**: `GET`

**Path**: `/v1/destinations/pre_trip_planner_ingress`

Returns trip planning UI configuration.

---

## Group Rides (v4)

### Group Scan Reserve

**Method**: `GET`

**Path**: `/v4/group_rides/views/bottom_sheets/group_scan_reserve`

Returns UI for group ride QR scanning and reservation.

---

## Deeplink Routes

The app handles the following deeplink routes:

| Deeplink | Parameters | Notes |
|----------|------------|-------|
| `limebike://map?selected_vehicle_id=XXX` | selected_vehicle_id, generated_at | Select vehicle on map |
| `limebike://receipt?trip_id=XXX` | trip_id | Trip receipt view |
| `limebike://help?trip_id=XXX` | trip_id | Help for specific trip |
| `limebike://limelab?hide_top_bar=true` | hide_top_bar | LimeLab features |
| `limebike://referral` | - | Referral program |
| `limebike://lime_wallet` | - | Wallet view |
| `limebike://donation` | - | Donation flow |
| `limebike://ride_history` | - | Ride history |
| `limebike://safety_center` | - | Safety resources |
| `limebike://settings` | - | App settings |
| `limebike://help_v2` | - | Help center |
| `limebike://payment_methods` | - | Payment methods |

---

The Juicer API can be found here: https://github.com/davidwim/lime-juicer/

## Implementations

* https://www.npmjs.com/package/@multicycles/lime (JavaScript)
