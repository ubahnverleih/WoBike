# Mobike

**Base url**: `https://mwx.mobike.com/mobike-api`

## Methods

### Get bicycles by lat/lng

**Method**: `POST`

**Path**: `/rent/nearbyBikesInfo.do`

**Header**:

| Header       | Value                                                                                                                                                                  | Mandatory |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------: |
| content-type | application/x-www-form-urlencoded                                                                                                                                      |     X     |
| user-agent   | Mozilla/5.0 (Linux; Android 7.0; SM-G892A Build/NRD90M; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/60.0.3112.107 Mobile Safari/537.36 (or whatever) |     X     |

**Parameters**:

| Parameters | Descriptions | Mandatory |
| ---------- | ------------ | :-------: |
| latitude   | latitude     |     X     |
| Longitude  | Longitude    |     X     |

**Exemple**

```bash
curl --request POST \
  --url https://mwx.mobike.com/mobike-api/rent/nearbyBikesInfo.do \
  --header 'content-type: application/x-www-form-urlencoded' \
  --header 'user-agent: Mozilla/5.0 (Linux; Android 7.0; SM-G892A Build/NRD90M; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/60.0.3112.107 Mobile Safari/537.36' \
  --data 'latitude=31.230390&longitude=121.473702'
```

```JSON
{
	"code": 0,
	"message": "",
	"biketype": 0,
	"autoZoom": true,
	"radius": 150,
	"object": [
		{
			"distId": "0216695456",
			"distX": 121.4741274074122,
			"distY": 31.229705282933747,
			"distNum": 1,
			"distance": "86",
			"bikeIds": "0216695456#",
			"biketype": 1,
			"type": 0,
			"boundary": null
		}
	]
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/mobike (JavaScript)
