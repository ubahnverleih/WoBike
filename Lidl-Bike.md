# Lidl-Bike

Lidl-Bike uses a RPC endpoint for their requests.
As of writing this, only one procedure is known, `listBikes`.

## Authentication
For listing their bikes, authentication is not required.

## RPC Endpoint

- Base URL: `https://www.lidl-bike.de/de/rpc`
- Method: `POST`
- Data: See tables below, has to be encoded as JSON.

### Root Object

Key      | Value(s)            | Description
---------|---------------------|-----------------------------------------------------
`method` | `Map.listBikes`     | The RPC method for listing bikes (only known so far)
`params` | List of parameters* | Parameters for the given method

### Params for `Map.listBikes` method

Param      | Value(s)      | Description
-----------|---------------|---------------------------------------------------
`lat`      | latitude      | Center latitude for bike search
`long`     | longitude     | Center longitude for bike search
`maxItems` | 0 < max < 100 | The number of results (limit is 100)
`radius`   | meters        | Radius around center in meters for the search
`name`     | `drag`        | This is mandatory. I have no idea what this means.

## Examples
### Get 100 bikes within 400 meters

```
curl --request POST \
    --url https://www.lidl-bike.de/de/rpc \
    --data '{"method":"Map.listBikes","params":[{"lat":52.52581526868782,"long":13.433961158935514,"maxItems":100,"radius":10000000000,"name":"drag"}]}'
```
