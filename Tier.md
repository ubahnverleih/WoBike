# Tier

[tier](https://www.tier.app/) is a scootersharing company based in Europe.

To authenticate you need to add `X-Api-Key: bpEUTJEBTf74oGRWxaIcW7aeZMzDDODe1yBoSxi2` in your header of the API-Endpoint `https://platform.tier-services.io`. You can request all(?) vehicles with `https://platform.tier-services.io/vehicle?` or filter them by zoneID like `https://platform.tier-services.io/vehicle?zoneId=BERLIN` or show all the vehicles around a location with a given radius (meters) `https://platform.tier-services.io/vehicle?lat=0&lng=0&radius=500`.

Polygons with regions and parking restrictions are available on `https://platform.tier-services.io/zone?lat=55.605&lng=13.0038`.
