# Spin (Bikes and Scooters)
[Spin](https://www.spin.pm/) is a Bike and E-Scotter sharing service in the US.

To Access the API, you need to generate an Auth token. This requires only a verifiable email address. Make a POST request to `https://web.spin.pm/api/v1/magic_links` with the body:
```
{"email": "<email>"}
```

From there, you will need to find the token within your email. This token needs to be sent with a POST request to `https://web.spin.pm/api/v1/auth_tokens` with the body:
```
{
    "grant_type": "magic_link",
    "magic_link": {
        "email": "<email>",
        "token": "<token>"
    }
}
```

This request returns a JSON that looks like this: `{"jwt":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyVW5pcXVlS2V5IjoiNDE2NWZmYmI0ZDVjODA0ODc5OTMzMTIwYzRiMGQ5NmYiLCJyZWZlcnJhbENvZGUiOm51bGwsImlzQXBwbGVQYXlEZWZhdWx0IjpmYWxzZSwiaXNBZG1pbiI6ZmFsc2UsImlzQ2hhcmdlciI6ZmFsc2UsImlzQ29ycG9yYXRlIjpmYWxzZSwiYXV0b1JlbG9hZCI6ZmFsc2UsImNyZWRpdEJhbGFuY2UiOjAsInRvdGFsVHJpcENvdW50IjowLCJzcGluVW5saW1pdGVkIjpmYWxzZSwic3BpblVubGltaXRlZE5leHRCaWxsaW5nQ3ljbGUiOm51bGwsInNwaW5VbmxpbWl0ZWRNZW1iZXJzaGlwIjpudWxsLCJpc1F1YWxpZmllZEZvclJpZGUiOmZhbHNlLCJyYXRlRGlzY291bnRQZXJjZW50YWdlIjowLCJ0eXBlIjoiZGV2aWNlIiwiZXhwIjoxNTI3NzkxMTMwfQ.p11U0XDfwGiI2HgM3m4FxQepFNmzBlouEdPHAGcXVLw","refreshToken":"2cf6fd9775313f975ef8b077abab5030","existingAccount":false}`

The important part is the `jwt`-Token. This Token is used for Authentication.
This tokens invalidates after some minutes.

To get the position of vehicles so a GET-Request to:
`https://web.spin.pm/api/v3/vehicles?lng=-77.0146489&lat=38.8969363&distance=&mode=`
User Header `Authorization`: `Bearer <JWT-TOKEN>` to Authenticate and use the `jwt`-Token we got from the Auth request.

You will get something like this as return JSON `{"vehicles":[{"lat":37.69247,"lng":-122.46595,"last4":"3595","vehicle_type":"bicycle","batt_percentage":null,"rebalance":null}, … ]}`
