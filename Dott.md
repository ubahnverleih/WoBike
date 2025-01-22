# Dott

## Introduction

Dott provides an API to access available cities by countries and information about scooters and bikes in those cities. The API uses the GBFS (General Bikeshare Feed Specification) format to provide standardized data.

## Table of Contents

1. [Get Available Cities by Countries](#get-available-cities-by-countries)
2. [Get Scooters and Bikes Information (GBFS)](#get-scooters-and-bikes-information-gbfs)
   - [Main GBFS URL](#main-gbfs-url)
   - [Get Vehicle Types](#get-vehicle-types)
   - [Get Prices by Vehicle Type](#get-prices-by-vehicle-type)
   - [Get Real-Time Vehicle Availability](#get-real-time-vehicle-availability)
   - [Get Geofencing Zones](#get-geofencing-zones)
   - [Get Vehicle Parking Locations](#get-vehicle-parking-locations)

## Get Available Cities by Countries

To get the list of available cities by countries, use the following endpoint:

**URL**: `https://gbfs.api.ridedott.com/public/v2/countries/{countries}/gbfs.json`

**Countries**: fr, de, ... (check the website for more countries)

---

## Get Scooters and Bikes Information (GBFS)

### Main GBFS URL

The main GBFS URL provides general information about the city's bike and scooter sharing system.

**URL**: `https://gbfs.api.ridedott.com/public/v2/{city}/gbfs.json`

**Parameter**:
- `city`: Provide the city name.

### Get Vehicle Types

To get the different types of vehicles available in the city (bike, scooter, etc.), use the following endpoint:

**URL**: `https://gbfs.api.ridedott.com/public/v2/{city}/vehicle_types.json`

**Method**: `GET`

### Get Prices by Vehicle Type

To get the pricing information for different vehicle types, use the following endpoint:

**URL**: `https://gbfs.api.ridedott.com/public/v2/{city}/system_pricing_plans.json`

**Method**: `GET`

### Get Real-Time Vehicle Availability

To get real-time information on available vehicles, use the following endpoint:

**URL**: `https://gbfs.api.ridedott.com/public/v2/{city}/free_bike_status.json`

**Method**: `GET`

### Get Geofencing Zones

To get the geofencing zones for the city, use the following endpoint:

**URL**: `https://gbfs.api.ridedott.com/public/v2/{city}/geofencing_zones.json`

**Method**: `GET`

### Get Vehicle Parking Locations

To get the parking locations for vehicles, use the following endpoint:

**URL**: `https://gbfs.api.ridedott.com/public/v2/{city}/station_information.json`

**Method**: `GET`
