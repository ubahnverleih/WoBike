# Coup

[Coup](https://www.joincoup.com/) was a rental service for electric motorbikes 🛵 (also called scooter, the wording is a mess).

First request the cities with `GET` request `https://app.joincoup.com/api/v3/markets`. Get the `id` for your city and request all vehicles with a `GET` request `https://app.joincoup.com/api/v3/markets/{id}/scooters` for example for Berlin: `https://app.joincoup.com/api/v3/markets/fb7aadac-bded-4321-9223-e3c30c5e3ba5/scooters`. A `GET` request to
`https://app.joincoup.com/api/v3/markets/{id}/business_areas` will give you business areas for that city.
