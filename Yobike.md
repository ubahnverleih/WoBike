# Yobike (ex ohbike)

**Base url**: `https://en.api.ohbike.com`

## Methods

### Signature

Requests should be "signed". We just need to add an `sign` attribute with an md5 hash.
To get the corresponding md5 hash:

* get all POST _values_ (excl. sign)
* sort the values
* concat the sorted values (ignoring empty values) to string
* add `@ohbile` on the end of the string

**Javascript exemple** (from apk)

```JavaScript
function getSignature(data) {
    var c = [],
        g, k;

    for (k in data) {
        g = data[k], void 0 == g && (g = "", data[k] = ""), c.push(g);
    }

    c.sort();
    data = "";

    for (k in c) {
        data += c[k];
    }

    return md5(data + "@ohbile");
}
```

### Get bicycles by lat/lng

**Method**: `POST`

**Path**: `/v1/vehicle/`

**Header**:

| Header       | Value                             | Mandatory |
| ------------ | --------------------------------- | :-------: |
| content-type | application/x-www-form-urlencoded |     X     |

**Parameters**:

| Parameters | Descriptions           | Mandatory |
| ---------- | ---------------------- | :-------: |
| lat        | Latitude               |     X     |
| lng        | Longitude              |     X     |
| distance   |                        |     X     |
| coord_type | Value: 1               |     X     |
| t          | value: "geonear"       |     X     |
| ts         | Timestamp in second    |     X     |
| ak         | Application key        |     X     |
| bounds     | Bounds around position |     X     |
| zoom       |                        |     X     |
| sign       | Signature (see above)  |     X     |

**Bounds**

Bounds parameter is a string of concatenated positions (southwest.latitude + "," + southwest.longitude + ";" + northeast.latitude + "," + northeast.longitude).

**Exemple**

```bash
curl --request POST \
  --url https://en.api.ohbike.com/v1/vehicle/ \
  --header 'content-type: multipart/form-data' \
  --form 'bounds=51.388098,-2.681621;51.578447,-2.509771' \
  --form uk= \
  --form ak=WWTaJQrg-NHe_Zl0iwghHyYypYw6g-6GEZHPGBBF6TI7OzZWo9VVLXWRs2ngQJ18 \
  --form t=geonear \
  --form distance=700 \
  --form ts=1515775926 \
  --form zoom=11.583380 \
  --form lat=51.483372 \
  --form sign=f9f22c789aaf4e997a44b64436e007cf \
  --form lng=-2.595696
```

:warning: Multiple requests with the same `ts` could cause errors.

```JSON
{
	"errcode": 0,
	"message": "ok",
	"data": [
		{
			"latitude": 51.44402,
			"longitude": -2.63239,
			"plate_no": "7701606",
			"outside": 0
		}
    ]
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/yobike (JavaScript)
