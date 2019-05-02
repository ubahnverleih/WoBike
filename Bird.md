# Bird (Scooters)

## Get Auth Token

To access the API, you have to generate an Auth Token. No worries, you don’t need an Account for this, just an POST-Request to `https://api.bird.co/user/login`. The POST Request-Body is the following JSON:
`{"email": "<EMAIL-ADDRESS>"}`.
And the Headers should include `Device-id`: `<GUID>`, `Platform`: `ios`, and `Content-type`: `application/json`
You don't need to be able to receive Mails on the address, but it needs to be unique. If you use an email address that was previously used by you or anyone else, you won't receive a `token`, only an `id`.

For the Device-Id you need to generate an random 16 Byte [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) like `123E4567-E89B-12D3-A456-426655440000`

As a result you get something like this: `{"id":"2b932653-b9e9-4bbe-a7e2-3d231b9877ba","token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJBVVRIIiwidXNlcl9pZCI6IjAzNjg0ODU2LTkwNmItNDY4OC04ODM3LTY1NTRkYzViOGVhMyIsImRldmljZV9pZCI6IjhCMUYwN0I0LTU5OEMtNDI0NS04REMxLTUzMTJFNjY4ODQyOSIsImV4cCI6MTU1OTQ2OTkxMH0.-UpZHkI9VMolxvvhjJc6qvQXkriynVQNm2PVZf63EQM"}`

We need the `token` for our location requests. This tokens invalidate after some time, so it could be you have to generate a new, or renew the token with the same request. (To renew do the same request with same request data, you don't get the token again, but an expire date).

## Request Location

Now you can request the locations of the Scooters:

Send a GET request to `https://api.bird.co/bird/nearby?latitude=37.77184&longitude=-122.40910&radius=1000`  
Set the following headers:

 * `Authorization`: `Bird <TOKEN>` – Use the token you got from Auth-Request.
 * `Device-id`: `<GUID>` – You can reuse the GUID from Auth-Request, but don't have to
 * `App-Version`: `3.0.5`
 * `Location`: `{"latitude":37.77249,"longitude":-122.40910,"altitude":500,"accuracy":100,"speed":-1,"heading":-1}` – Yes this is JSON in a header ;) – You should use the same data like from the GET request params.

 The Result looks like this: `{"birds":[{"id":"1486a00c-fd73-4370-9250-782f5c60ee2d","code":"6JLE","location":{"latitude":37.77216,"longitude":-122.409485},"battery_level":89}, ... ]}`
 
## Request Configuration

Send a GET request to `https://api.bird.co/config/location?latitude=42.3140089&longitude=-71.2490943`.  
The only needed header is `App-Version: 3.0.5`.

The result contains the configuration for the app in this specific location, including stuff like `weather_alert` and service periods and `current_service_status`.

## Libraries

 * https://github.com/jzarca01/node-bird
