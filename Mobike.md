# Mobike

**Base url**: `http://app.mobike.com/api`

## Methods

### Get bicycles by lat/lng

**Method**: `POST`

**Path**: `/nearby/v4/nearbyBikeInfo`

**Header**:

| Header       | Value                                                                                                                                                                  | Mandatory |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------: |
| content-type | application/x-www-form-urlencoded                                                                                                                                      |     X     |
| platform     | 1                                                                                                                                                                      |     X     |
| user-agent   | Mozilla/5.0 (Android 7.1.2; Pixel Build/NHG47Q) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.9 NTENTBrowser/3.7.0.496 (IWireless-US) Mobile Safari/537.36 (or whatever) |     X     |

**Parameters**:

| Parameters | Descriptions | Mandatory |
| ---------- | ------------ | :-------: |
| latitude   | latitude     |     X     |
| Longitude  | Longitude    |     X     |

**Example**

```bash
curl --request POST \
     --url http://global-n-mobike-g.mobike.com/api/nearby/v4/nearbyBikeInfo \
     --header 'platform: 1' \
     --header 'Content-Type: application/x-www-form-urlencoded' \
     --header 'User-Agent: Mozilla/5.0 (Android 7.1.2; Pixel Build/NHG47Q) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.9 NTENTBrowser/3.7.0.496 (IWireless-US) Mobile Safari/537.36' \
     --data 'latitude=52.53&longitude=13.38'
```

```JSON
{
  "code": 0,
  "message": "",
  "bike": [
    {
      "distId": "A816046038",
      "distX": 13.379844,
      "distY": 52.529358,
      "distNum": 1,
      "distance": "72",
      "bikeIds": "A816046038#",
      "biketype": 2,
      "type": 0,
      "boundary": null,
      "operateType": 2
    }
  ],
  "mpl": [],
  "biketype": 0,
  "radius": 500,
  "autoZoom": false,
  "hasRedPacket": 0
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/mobike (JavaScript)
