# Yobike and yobike-based provider 

Yobike (ex ohbike) sell their systems under white label.
Only the Application key differ

**Base url**: `https://en.api.ohbike.com`

Yobike (ex ohbike) sell their systems under white label.
Only the Application key differ

| Brand        | Application key                                                  |
| ------------ | ---------------------------------------------------------------- |
| Yobike       | WWTaJQrg-NHe_Zl0iwghHyYypYw6g-6GEZHPGBBF6TI7OzZWo9VVLXWRs2ngQJ18 |
| Indigo Wheel | NaDl8eR81njaT7FRMNn2oqH020bAfUG7d7Iqa2kMvZm8qCga5cg_QIlk_XZVZvWI |

## Methods

### Signature

Requests should be "signed". We just need to add an `sign` attribute with an md5 hash.
To get the corresponding md5 hash:

* get all POST _values_ (excl. sign)
* sort the values
* concat the sorted values (ignoring empty values) to string
* add `@ohbile` on the end of the string

**Javascript example** (from apk)

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

**Python Example** (adapted from JS)

```python3
import hashlib

def get_signature(data):
  relevant_values = [str(v)
                     for (k, v) in query_data.items()
                     if v and k != 'sign']
  hash_input = ''.join(sorted(relevant_values))
  hash_input += '@ohbile'
  md5 = hashlib.md5(hash_input.encode('ascii')).hexdigest()
return md5
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

**Bash/Curl Example**

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

**Python Example**

```python3
import json
import time
import requests

# for indigoweel
app_key = 'NaDl8eR81njaT7FRMNn2oqH020bAfUG7d7Iqa2kMvZm8qCga5cg_QIlk_XZVZvWI'

# for yobiki
# app_key = 'WWTaJQrg-NHe_Zl0iwghHyYypYw6g-6GEZHPGBBF6TI7OzZWo9VVLXWRs2ngQJ18'

def prep_query(lat_sw, long_sw, lat_ne, long_ne):
  lat_mean = str((lat_sw + lat_ne) / 2)
  long_mean = str((long_sw + long_ne) / 2)
  query_data = {
    "lat": lat_mean,
    "lng": long_mean,
    "distance": "100000",
    "t": "geonear",
    "ts": str(int(time.time())),
    "ak": app_key,
    "zoom": "11.583380",
    "bounds": f'{lat_sw},{long_sw};{lat_ne},{long_ne}'
  }
  return query_data

# all of grenoble
query_data = prep_query(45.140106, 5.668827, 45.211247, 5.794435)

# just around prefecture verdun
# query_data = prep_query(45.18850600964806, 5.730357170104981, 45.18925458649066, 5.731741189956665)

query_data['sign'] = get_signature(query_data)

print('='*80)
print('REQUEST:')
print('='*80)
print(json.dumps(query_data, indent=2))
print('='*80)

headers = {'content-type': 'multipart/form-data'}
headers = {'content-type': 'application/x-www-form-urlencoded'}
req = requests.post('https://en.api.ohbike.com/v1/vehicle/',
                    headers=headers,
                    data=query_data)
bikes = json.loads(req.text)
response_json = json.dumps(bikes, indent=2)
print()
print('='*80)
print('RESPONSE:')
print('='*80)
try:
  from pygments import highlight, lexers, formatters
  colorful_json = highlight(response_json, lexers.JsonLexer(), formatters.TerminalFormatter())
  print(colorful_json)
except:
  print(response_json)
print('='*80)
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
