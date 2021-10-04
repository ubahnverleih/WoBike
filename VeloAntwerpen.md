# Velo Antwerpen
[Velo Antwerpen](https://velo-antwerpen.be) is a bike sharing service that operates in Antwerp. The technology used is the Clear Channel SmartBike system.

**Base URL**: `https://www.velo-antwerpen.be/availability_map`

## Get stations with available bikes and free slots
**Path**: `/getJsonObject`

**Method**: `GET`

Response:
```
[
  {
    "address": "Koningin Astridplein",
    "bikes": "27",
    "id": "001",
    "lat": "51.217820000000000000",
    "lon": "4.420650000000000000",
    "name": "001- Centraal Station - Astrid",
    "slots": "6",
    "stationType": "BIKE",
    "status": "OPN"
  },
  {
    "address": "Koningin Astridplein",
    "bikes": "1",
    "id": "002",
    "lat": "51.21755807739536",
    "lon": "4.420725651433662",
    "name": "002- Centraal Station - Astrid 2",
    "slots": "34",
    "stationType": "BIKE",
    "status": "OPN"
  },
  {
    "address": "Kievitplein",
    "bikes": "8",
    "id": "003",
    "lat": "51.213085853125690000",
    "lon": "4.421771338842944000",
    "name": "003- Centraal station Kievit 2",
    "slots": "22",
    "stationType": "BIKE",
    "status": "OPN"
  },
  {
    "address": "De Keyserlei",
    "bikes": "0",
    "id": "004",
    "lat": "51.217363888888890000",
    "lon": "4.420094444444445000",
    "name": "004- De Keyserlei",
    "slots": "21",
    "stationType": "BIKE",
    "status": "OPN"
  },
  {
    "address": "Lange Kievitstraat",
    "bikes": "12",
    "id": "005",
    "lat": "51.213010729744000000",
    "lon": "4.421732916607050000",
    "name": "005- Centraal Station / Kievit",
    "slots": "24",
    "stationType": "BIKE",
    "status": "OPN"
  }
 ]
```
