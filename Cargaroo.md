# Cargaroo
[Cargaroo](https://cargoroo.nl/en/) is an dutch e-cargobike sharing company.

### Get Stations
```GET https://api.cargoroo.eu/v1/stations/assets/?_limit=50&_offset=0&_sort=distance&radius=408394&latitude=52.487&longitude=13.393```
Returns bike stations with all the available vehicles.

| URL Param | Explanation |
|-----------|-------------|
| _limit    | How many stations are returned |
| _offset   | Page through stations - usually n*offset |
| _sort     | Sort by distance |
| radius    | Search radius - not sure if this works as intended |
| latitude  | Latitude |
| longitude | Longitude |

### Info on Bike
```GET https://api.cargoroo.eu/v1/assets/BE-212/```  
Get info on Bike. Replace `BE-212` with the bike ID you want to get info about.
