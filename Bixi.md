# Bixi (Montréal, QC, Canada)

A GET request to:

```
https://layer.bicyclesharing.net/map/v1/mtl/stations
```

yields a response looking like:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -73.55650842189789,
          45.51035067563653
        ]
      },
      "properties": {
        "station_id": "1",
        "name": "Métro Champ-de-Mars (Sanguinet / Viger)",
        "terminal": "6001",
        "capacity": 33,
        "bikes_available": 11,
        "docks_available": 22,
        "bikes_disabled": 0,
        "docks_disabled": 0,
        "renting": true,
        "returning": true,
        "installed": true,
        "last_reported": 1533227391,
        "icon_pin_bike_layer": "pin-bike-green-half",
        "icon_pin_dock_layer": "pin-dock-green-most",
        "icon_dot_bike_layer": "dot-green",
        "icon_dot_dock_layer": "dot-green",
        "valet_status": "none"
      }
    }
  ]
}
```

The array of "Features" contains status on each Bixi bike sharing station in Montréal.
