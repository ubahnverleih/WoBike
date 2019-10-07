# EnTur (Aggregate endpoint, Norway)
The Entur endpoint is an aggregate endpoint that returns position of scooters in Norway. 
Examples of supported companies in the Oslo area are Zvipp, Tier and Voi. However the endpoint is not limited to Oslo. 

## Requesting Data
### HTTP GET: 
Example get request:
`GET https://api.entur.io/mobility/v1/scooters?lat=59.909&lon=10.746&range=200&max=20
`
### Parameters: 
|Parameter  |Description | 
|--|--|
| lat | Latitude|
|lon|Longitude|
|range|Radius in meters (optional), 200 default|
|max| Maximum amount of scooters (optional), 20 default| 

## Example response 
```json
[
  {
    "id": "cec80aa5-5497-4b82-b186-6bccab7c87e6",
    "operator": "voi",
    "lat": 59.908581,
    "lon": 10.745911,
    "code": "-",
    "battery": 70
  },
  {
    "id": "10fc0759-20ec-4512ab8dd1b3-1ca9adcb",
    "operator": "zvipp",
    "lat": 59.910015,
    "lon": 10.747366,
    "code": "ZVS1332",
    "battery": 33
  },
  {
    "id": "10fc0759-20ec-4512ab8dd1b3-1ca9adcb",
    "operator": "zvipp",
    "lat": 59.910015,
    "lon": 10.747366,
    "code": "ZVS1332",
    "battery": 33
  }
]
``` 

## Source
More information about EnTurs endpoints can be found at:
https://developer.entur.org/
and at:
https://github.com/entur
