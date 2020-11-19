# stella

[stella](https://www.ridestella.com) is a scootersharing company based in Europe.

There is *no* authentication or special headers and only `GET`-Requests required for the API-Endpoint `https://stella.frontend.fleetbird.eu/api/prod/v1.06/`. You can request all(?) vehicles with `https://stella.frontend.fleetbird.eu/api/prod/v1.06/map/cars/` or filter them by bbox like `https://stella.frontend.fleetbird.eu/api/prod/v1.06/map/cars/?lat1=48.5&lat2=49.5&lon1=8.5&lon2=9.5` or show the 20 nearest vehicles around a location with `https://stella.frontend.fleetbird.eu/api/prod/v1.06/cars/?lat=48.8&lon=9.1`.

Polygons with regions and parking restrictions are available on `https://stella.frontend.fleetbird.eu/api/prod/v1.06/territories/all/` (`type: 0` looks like free floating region and `type: 1` are no parking zones).

Vehicle types with images are available on `https://stella.frontend.fleetbird.eu/api/prod/v1.06/cars/types/`
