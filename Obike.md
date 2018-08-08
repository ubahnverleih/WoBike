# Obike

**Base url**: `https://mobile.o.bike/api/v2`

In this documentation we use the `3.2.4` (or `324`) version.

`oBiOSX4buhBMG` and `oBiOSMYFUzLed` are constantes

## Methods

### Get bicycles by lat/lng

**Method**: `POST`

**Path**: `/bike/list`

**Header**:

| Header        | Value                                              | Mandatory |
| ------------- | -------------------------------------------------- | :-------: |
| content-type  | application/json                                   |     X     |
| version       | 3.2.4                                              |     X     |
| platform      | iOS                                                |     X     |
| User-Agent    | DOTCBike/${3.2.4} (iPhone; iOS 11.2.6; Scale/2.00) |           |
| Authorization |                                                    |           |

**Parameters**:

:warning: Body parameters should be encoded and set body `value` field :warning:

| Parameters  | Descriptions                                        | Mandatory |
| ----------- | --------------------------------------------------- | :-------: |
| latitude    | Latitude                                            |     X     |
| longitude   | Longitude                                           |     X     |
| countryCode | 49 (should be an other code, doesn't really matter) |     X     |
| deviceId    | User token (get by auth)                            |           |
| dateTime    | Timestamp + '.000000'                               |           |

**Encoding**

1.  JSON stringify data, remove all spaces and line breaks
2.  concat (<result of 1>,`&oBiOSX4buhBMG324`)
3.  concat (<result of 1>,'&',SHA1(<result of 2>))
4.  encode the result of 3 in AES128 with `oBiOSMYFUzLed324` as key and `1234567890123456` as IV and return it in hex

**Example**

with simple data

```JSON
{
    "countryCode": "33",
    "longitude": 2.369336,
    "latitude": 48.852775
}
```

```bash
curl --request POST \
  --url https://mobile.o.bike/api/v2/bike/list \
  --header 'content-type: application/json' \
  --header 'platform: iOS' \
  --header 'version: 3.2.4' \
  --data '{ "value": "1152a752ff6623d245263cc1b500d38306d974dbf57110d8ace721bbaa34511e540976066d0d2d419172a34d7e1283b277d6a17091fd79d38e919ece60608c841378fef75ddf9f434067c768e88983d516a79ef7645c5d0150a3252da11ae30e20a74c8e7243c0fa1f6504536542c43c" }'
```

```JSON
{
	"data": {
		"iconUrl": null,
		"list": [
			{
				"id": "033004123",
				"longitude": 2.371797,
				"latitude": 48.853308,
				"imei": "3166756E63746937",
				"iconUrl": null,
				"promotionActivityType": null,
				"rideMinutes": null,
				"countryId": 55,
				"cityId": 250,
				"helmet": 0
			}
		]
	},
	"success": true,
	"errorCode": 100
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/obike (JavaScript)
