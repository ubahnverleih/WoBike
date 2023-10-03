# Hive 

[hive](https://www.ridehive.com) is a scootersharing company based in Europe.

There is *no* authentication or special headers and only `GET`-Requests required for the API-Endpoint `https://hive.frontend.fleetbird.eu/api/prod/v1.06/`. You can request all(?) vehicles with `https://hive.frontend.fleetbird.eu/api/prod/v1.06/map/cars/` or filter them by bbox like `https://hive.frontend.fleetbird.eu/api/prod/v1.06/map/cars/?lat1=46.8339&lat2=55.829&lon1=4.205&lon2=27.653` or show the 20 nearest vehicles around a location with `https://hive.frontend.fleetbird.eu/api/prod/v1.06/cars/?lat=53.4374&lon=9.9955`.

Polygons with regions and parking restrictions are available on `https://hive.frontend.fleetbird.eu/api/prod/v1.06/territories/all/` (`type: 0` looks like free floating region and `type: 1` are no parking zones).

Vehicle types with images are available on `https://hive.frontend.fleetbird.eu/api/prod/v1.06/cars/types/`
