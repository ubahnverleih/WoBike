# Ufo (defunct)

[Ufo](https://www.ufoscooters.com/) was a scootersharing company based in Europe.

There is *no* authentication or special headers and only `GET`-Requests required for the API-Endpoint `https://ufo.frontend.fleetbird.eu/api/prod/v1.06/`. You can request all visible  vehicles with `https://ufo.frontend.fleetbird.eu/api/prod/v1.06/map/cars/` or filter them by bbox like `https://ufo.frontend.fleetbird.eu/api/prod/v1.06/map/cars/?lat1=35.0&lat2=44.0&lon1=-35.0&lon2=4.0` or show the 20 nearest vehicles around a location with `https://ufo.frontend.fleetbird.eu/api/prod/v1.06/cars/?lat=40.4021&lon=-3.6857`.

Polygons with regions and parking restrictions are available on https://ufo.frontend.fleetbird.eu/api/prod/v1.06/territories/all/ (type: 0 looks like free floating region and type: 1 are no parking zones).

Vehicle types with images are available on https://ufo.frontend.fleetbird.eu/api/prod/v1.06/cars/types/
