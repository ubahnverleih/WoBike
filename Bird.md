# Bird (Scooters)

To Access API, you have to generate an Auth token. No worries, you don’t need an Account for this, just an POST-Request to `https://api.bird.co/user/login`. The POST Request-Body is the following JSON:
`{"email": "<EMAIL-ADDRESS>"}`.
And the Headers should include `Device-id`: `<GUID>`, `Platform`: `ios`, and `Content-type`: `application/json`
The E-Mail-Address can be everything that looks like an E-Mail-Address. You don't need to be able to recive Mails on this address.
For the Device-Id you need to generate an random 16 Byte [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) like `123E4567-E89B-12D3-A456-426655440000`

As a result you get something like this: `{"id":"2b932653-b9e9-4bbe-a7e2-3d231b9877ba","token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJBVVRIIiwidXNlcl9pZCI6IjAzNjg0ODU2LTkwNmItNDY4OC04ODM3LTY1NTRkYzViOGVhMyIsImRldmljZV9pZCI6IjhCMUYwN0I0LTU5OEMtNDI0NS04REMxLTUzMTJFNjY4ODQyOSIsImV4cCI6MTU1OTQ2OTkxMH0.-UpZHkI9VMolxvvhjJc6qvQXkriynVQNm2PVZf63EQM"}`

We need the `token` for our location requests. This tokens invalidate after some time, so it could be you have to generate a new, or renew the token with the same request. (To renew do the same Requst with same request data, you don't get the token again, but an expire date).

Now you can request the locations of the Scooters:

GET-Request to `https://api.bird.co/bird/nearby?latitude=37.77184&longitude=-122.40910&radius=1000`
And set the following headers:

 * `Authorization`: `Bird <TOKEN>` – Use the token you got from Auth-Request.
 * `Device-id`: `<GUID>` – You can reuse the GUID from Auth-Request, but don't have to
 * `App-Version`: `3.0.5`
 * `Location`: `{"latitude":37.77249,"longitude":-122.40910,"altitude":500,"accuracy":100,"speed":-1,"heading":-1}` – Yes this is JSON in a header ;) – You should use the same data like from the get Request Params.

 The Result looks like this: `{"birds":[{"id":"1486a00c-fd73-4370-9250-782f5c60ee2d","code":"6JLE","location":{"latitude":37.77216,"longitude":-122.409485},"battery_level":89}, ... ]}`
