# Flamingo
[Flamingo](https://www.flamingoscooters.com/) is a E-Scooter sharing service that operates in New Zealand.

**URL**: `https://api.flamingoscooters.com/gbfs/<city>/free_bike_status.json`

Replace the `<city>` in the URL with either: `Auckland`, `Christchurch` or `Wellington`.

The output should be something like this:

```
{
                "bike_id": "1339",
                "lat": -43.532093,
                "lon": 172.628018,
                "current_range_meters": 11900,
                "last_reported": 1599977150
            },
```

## Meanings from output

`bike_id`: The code that lets riders identify the scooter and unlock it

`lat`: Latitude location of scooter

`lon`: Longitude location of scooter

`current_range_meters`: Battery level of scooter in meters

`last_reported`: The date and time for when the scooter last sent a GPS Ping, this would need to be decoded
