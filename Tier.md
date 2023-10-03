
# Tier

[Tier](https://www.tier.app/) is a scootersharing company based in Europe.

To authenticate you need to add `X-Api-Key: bpEUTJEBTf74oGRWxaIcW7aeZMzDDODe1yBoSxi2` in your header of the API-Endpoint `https://platform.tier-services.io`.  

Official docs: https://api-documentation.tier-services.io/

## v1

### Get Vehicles
Vehicles within a range: `GET https://platform.tier-services.io/v1/vehicle?lat=48.1&lng=16.3&radius=5000`
Vehicles within a zone: `GET https://platform.tier-services.io/v1/vehicle?zoneId=BERLIN`
Vehicles by Code (encoded in the vehicle QR-code): `GET https://platform.tier-services.io/v1/vehicle/code/12345`
Vehicles by UUID: `GET https://platform.tier-services.io/v1/vehicle/<UUID>`

### Make vehicle flash
`POST https://platform.tier-services.io/v1/vehicle/<vehicle-id>/flash`

### Get vehicles with battery swap option
This endpoint needs an X-Firebase-Auth: Bearer instead of X-Api-Key
`GET https://platform.tier-services.io/v1/user-battery-swap/config?lat=<lat>&lng=<lng>&radius=<radius>`

### Zones
Get zones by type: `GET https://platform.tier-services.io/v1/zone?type=<zone-type>`
Get subzones by type: `GET https://platform.tier-services.io/v1/zone/BERLIN/subzone?type=constrained`

Zone Types:
|Parameter| Description |
|--|--|
| `root` | The zone to which all other zone types are attached, e.g. BERLIN |
| `business` | An area in which customers can rent a vehicle |
| `warehouse` | An area in which scooters are in MAINTENANCE |
| `constrained` | An area which may not allow parking or only allow reduced speed or both (may overlap with the business zone) |An area which may not allow parking or only allow reduced speed or both (may overlap with the business zone)|
| `parking` | An area where customers may park their vehicle only in designated areas.

Get zones near location: `GET https://platform.tier-services.io/v1/zone?lat=<latitude>&lng=<longitude>`
Validate zone for parking: `GET https://platform.tier-services.io/v1/zone/validate-constraint?lat=<latitude>&lng=<longitude>`


### Pricing
`GET https://platform.tier-services.io/v2/pricing?vehicleId=52911ecb-60a4-2535-2875-fb443eb5409f`


## v2

Only difference seems to be `emoped` as an additional vehicle type.
Get all vehicles within a zone: `GET https://platform.tier-services.io/v2/vehicle?zoneId=BERLIN`

### Get current subscription
This endpoint needs an X-Firebase-Auth: Bearer instead of X-Api-Key
`GET https://platform.tier-services.io/v2/subscription`

## v3
Passes and flatrate offers are provided at a v3 API endpoint but not at the /v2/pricing endpoint. All offers available in a zone are at /article. For this endpoint you need to provide a X-Firebase-Auth: Bearer header instead of a X-Api-Key.
`GET https://platform.tier-services.io/v3/article?zoneId=<zone>`
