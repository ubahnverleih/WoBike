# Bird (Scooters)
[Bird](https://www.bird.co/) is a E-Scotter sharing service in the US.

## Get Auth Token

First, you need to get authorization. This requires a verifiable email and a GUID. Send a POST request to `https://api-auth.prod.birdapp.com/api/v1/auth/email` with the body:
```
{"email":"<EMAIL>"}
```
And the Headers should include
```
User-Agent: Bird/4.53.0 (co.bird.Ride; build:24; iOS 12.4.1) Alamofire/4.53.0
Device-Id: <GUID>
Platform: ios
App-Version: 4.53.0
Content-Type: application/json
```

For the Device-Id you need to generate an random 16 Byte [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) like `123E4567-E89B-12D3-A456-426655440000`


Then, you'll need to find the token in your email and send that in a POST to `https://api-auth.prod.birdapp.com/api/v1/auth/magic-link/use`

The body:
```
{"token":"<TOKEN>"}
```

As a result you get something like this:
```
{
    "access": "<LONG STRING>",
    "refresh": "<LONG STRING>"
}
```
## Refresh Auth Token

By default bird tokens expire after one day. They can be easily refreshed without having to get another magic link. Send a POST request to `https://api-auth.prod.birdapp.com/api/v1/auth/refresh/token` with the following headers:
```
User-Agent: Bird/4.53.0 (co.bird.Ride; build:24; iOS 12.4.1) Alamofire/4.53.0
Device-Id: <GUID>
Platform: ios
App-Version: 4.53.0
Content-Type: application/json
Authorization: Bearer <REFRESH TOKEN YOU ALREADY HAVE>
```
Note, authorization header is Bearer and not Bird
As a result youll get two new tokens: access and refresh

## Request Location

Now you can request the locations of the Scooters:

Send a GET request to `https://api.birdapp.com/bird/nearby?latitude=37.77184&longitude=-122.40910&radius=1000`
Set the following headers:

 * `Authorization`: `Bird <TOKEN>` – Use the Access token you got from Auth-Request.
 * `Device-id`: `<GUID>` – You can reuse the GUID from Auth-Request, but don't have to
 * `App-Version`: `4.41.0`
 * `Location`: `{"latitude":37.77249,"longitude":-122.40910,"altitude":500,"accuracy":100,"speed":-1,"heading":-1}` – Yes this is JSON in a header ;) – You should use the same data like from the GET request params.

 The Result looks like this: `{"birds":[{"id":"1486a00c-fd73-4370-9250-782f5c60ee2d","code":"6JLE","location":{"latitude":37.77216,"longitude":-122.409485},"battery_level":89}, ... ]}`

## Request Areas

You can request no-parking zones or operational service areas by sending a GET request to `https://api.birdapp.com/bird/nearby?latitude=37.77184&longitude=-122.40910&radius=1000`

This endpoint requires the same headers as the `Request Location` endpoint above.

## Request Configuration

Send a GET request to `https://api.birdapp.com/config/location?latitude=42.3140089&longitude=-71.2490943`.
The only needed header is `App-Version: 4.41.0`.

The result contains the configuration for the app in this specific location, including stuff like `weather_alert` and service periods and `current_service_status`.

## Libraries

 * https://github.com/jzarca01/node-bird
