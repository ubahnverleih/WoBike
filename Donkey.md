# Donkey Republic

- Base URL: `https://stables.donkey.bike/api/public/`
- Header: `"Accept": "application/com.donkeyrepublic.v7"`

## Authentication
For the endpoints known so far, no authentication is required.

## Endpoints
As of writing this, only endpoints getting information are known.

### Get hubs
Hubs are the gathering points, where the vehicles wait to be rented.

- Base URL: `nearby`
- Description: List all hubs within a bounding box

Parameter     | Value(s)        | Description
--------------|-----------------|-------------------------------------------
`top_right`   | `ne_lat,ne_lon` | North-East coordinates of the bounding box
`bottom_left` | `sw_lat,sw_lon` | South-West coordinates of the bounding box
`filter_type` | `box`           | Unknown, but mandatory.

#### Example
```
curl -X GET \
  --header 'Accept:application/com.donkeyrepublic.v7' \
  "https://stables.donkey.bike/api/public/nearby?top_right=52.51778%2C13.38200&bottom_left=52.51509%2C13.37627&filter_type=box"
```

### Get more information about hub

- Base URL: `hubs/{HUB_ID}/{type}`, where `HUB_ID` is the ID from the `Get hubs` endpoint and `type` is either `bike`, `ebike`, `escooter` or `trailer`.

#### Example
```
curl -X GET \
  --header 'Accept:application/com.donkeyrepublic.v7' \
  "https://stables.donkey.bike/api/public/hubs/hub_6004/bike"
```
