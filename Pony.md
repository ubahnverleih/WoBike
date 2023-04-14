
<div style="padding: 20px;">
  <img src="https://pbs.twimg.com/profile_images/958669286733766656/mojuRBdB_400x400.jpg" alt="Pony logo" style="display: block; margin: auto; width: 100px;">
</div>

# Pony

**Base URL**: `https://api.getapony.com`

# Cities (aka regions)
## Get nearest region

**Path**: `/georegion/resolve`

**Method**: `GET`

**Parameters**:

| Parameters | Value                    | Mandatory |
| ---------- | ------------------------ | :-------: |
| latitude   | latitude                 | X         |
| longitude  | longitude                | X         |

## Get regions list

**Path**: `/georegion/all/`


**Method**: `GET`

---
# Get scooters and bikes informations (GBFS)

## Main GBFS url
`https://gbfs.getapony.com/v1/{city}/en/gbfs.json`

`city` : provide city name (aka region)

## Get the different types of vehicles available in the city (bike, scooter, ...)

**URL**: `https://gbfs.getapony.com/v1/{city}/en/vehicle_types.json`

**Method**: `GET`
## Get prices by vehicle type

**URL**: `https://gbfs.getapony.com/v1/{city}/en/system_pricing_plans.json`


**Method**: `GET`
## Get real-time information on available vehicles 

**URL**: `https://gbfs.getapony.com/v1/{city}/en/free_bike_status.json`

**Method**: `GET`

## Get geofencing zones

**URL**: `https://gbfs.getapony.com/v1/{city}/en/geofencing_zones.json`

**Method**: `GET`
## Get vehicle parking locations

**URL**: `https://gbfs.getapony.com/v1/{city}/en/station_information.json`

**Method**: `GET`
