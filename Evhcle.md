# Evhcle

[Evhcle](https://evhcle.com/) is an custom rental system for companys and communitys (e.g. hotels or villages).

There is *no* authentication or special headers, and only `GET`-Requests.


## Directory
Get (some) available endpoints.

```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/

{
    "cars": "https://evhcle.frontend.fleetbird.eu/api/prod/v1.06/cars/",
    "map/cars": "https://evhcle.frontend.fleetbird.eu/api/prod/v1.06/map/cars/",
    "locations": "https://evhcle.frontend.fleetbird.eu/api/prod/v1.06/locations/"
}
```
The version number does not appear to affect the actual data returned, however as the app uses 3.06 well stick with that.

## Territories
Get buisness-areas.
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/territories/all
```
`type 0` is the free floating parking zone. This zone is usually just a few meters arround the company cooperating with evhcle.

## Vehicle locations
Request available vehicles. Detailed information but no filtering. See below.
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/cars/
```

## Vehicle locations (filtering)
Request available vehicles. 
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/map/cars
```

You can filter the returned vehicles:
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/map/cars/?lat1=48.1&lat2=48.2&lon1=11.4&lon2=11.5
```

# Get vehicle
Get information about an car by id.
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/cars/{ID}
```
For example:
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/cars/51
```

## Vehicle Types
Returns vehicle types with images.
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/cars/types
```

## Locations
All citys/zones/companys they are in/cooperate with.
```
GET https://evhcle.frontend.fleetbird.eu/api/prod/v3.02/locations
```
