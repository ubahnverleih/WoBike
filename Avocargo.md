# Avocargo (Bankrupt)

[Avocargo](https://www.avocargo.one/) is a cargo-bike sharing company based in Germany.

There is *no* authentication or special headers, and only `GET`-Requests.


## Directory
Get (some) available endpoints.

```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/

{
  "cars": "https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/cars/",
  "map/cars": "https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/map/cars/",
  "locations": "https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/locations/"
}
```

## Territories
Get no-parking zones.
```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/territories/all/
```
Result see [here](https://gist.github.com/BastelPichi/ffbc239707fbf08390e35c99d1b5f6e3). (12.12.2022)

`type 0` looks like free floating region, `type: 1` menas no parking zone.

## Vehicle locations
Request available vehicles. Detailed information but no filtering. See below.
```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/cars/
```
Result see [here](https://gist.github.com/BastelPichi/dd2dc84299dfad495f64523cd00d1017). (12.12.2022)

## Vehicle locations (filtering)
Request available vehicles. 
```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/map/cars
```
Result see [here](https://gist.github.com/BastelPichi/a7ecd669f586b73bd90e19d7c8b3837f). (12.12.2022)

You can filter the returned vehicles:
```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/map/cars/?lat1=52.3&lat2=52.5&lon1=13.1&lon2=13.4
```
Result see [here](https://gist.github.com/BastelPichi/260d5bc5620bb2aaecc201766c6347b6). (12.12.2022)

## Vehicle Types
Returns vehicle types with images.
```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/cars/types
```
Result see [here](https://gist.github.com/BastelPichi/aba977b45acdfdb03496b3dbba59fad9). (12.12.2022)

## Locations
Unclear what this does. Maybe their warehouses?
```
GET https://avocargo.frontend.fleetbird.eu/api/prod/v1.06/locations/
```
Result see [here](https://gist.github.com/BastelPichi/d3b5ae49173fdcd1aeabae26fe29d8b4). (12.12.2022)
