# Baqme (E-cargo bike)
[Baqme](https://www.baqme.com/en/) is an e-cargo bike provider in The Netherlands.

## Vehicles

Send a `POST` request to this URL:

```
https://baqmeconnector-api.joyridecity.bike/api/v1/map/get-markers
```

With the following URL parameters:

- latitude
- longitude
- radiusInKm

E.g.: 

```
 curl -X POST 'https://baqmeconnector-api.joyridecity.bike/api/v1/map/get-markers?latitude=52.078663&longitude=4.288788&radiusInKm=10'
```