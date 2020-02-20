# WIND (former BYKE)

Simple GET-Request example: `https://api-prod.ibyke.io/v2/bikes?latitude=52.55001&longitude=13.40902&order=nearby`

With `https://api-prod.ibyke.io/v2/parkingPorts?latitude=52.55001&longitude=13.40902`, you can get the parking zones or parking ports for a location. However, that method requires authentication with a clientId and a userId.

A simple GET request to `https://api-prod.ibyke.io/v2/bikes/7aad7d2a-4fd9-48cf-992b-1057471ec208`, where the ID can be replaced with any other WIND bike or scooter ID returns the status of a specific scooter or bike.
