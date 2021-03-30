[Neuron](https://rideneuron.com/) is a scooter sharing service that operates worldwide.

Base URL: https://au-api.neuron-mobility.com - for Australia

Base URL: https://nz-api.neuron-mobility.com - for New Zealand

Base URL: https://sg-api.neuron-mobility.com - for Singapore

Base URL: https://kr-api.neuron-mobility.com - for Korea

Base URL: https://gb-api.neuron-mobility.com - for UK

The API uses a custom `sign` signature header. This is built by using the GET/POST parameters, the `timestamp` header and a secret.

```python
import hashlib
import urllib.parse
from collections import OrderedDict

# get params or post body
params = {"distance":"335","latitude":"-27.469746172603777","longitude":"153.02297267664835"}
# add timestamp from header
params["timestamp"] = "1616942614041"

secret = "neuron(!@#2018^&*)mobility"

params = OrderedDict(sorted(params.items()))
url_params = urllib.parse.urlencode(params, doseq=True, quote_via=lambda x, *_: x)
hash_input = url_params + "#" + secret
digest = hashlib.md5(hash_input.encode())
print(digest.hexdigest())
```

To get the API endpoints:
```
GET https://au-api.neuron-mobility.com/api/public/countries

timestamp: 1616942614041
sign: 261492ea9bbf5bafca7ddc6dbba86c5d
```

<details>

```
{
  "data": [
    {
      "name": "Singapore",
      "numeric": 702,
      "code": "SG",
      "digitalLines": [],
      "servers": {
        "pro": "https://sg-api.neuron-mobility.com"
      },
      "cities": [
        {
          "name": "Singapore",
          "code": "SG",
          "latitude": 1.313996,
          "longitude": 103.704164,
          ...
        }
      ],
      ...
    },
    {
      "name": "Australia",
      "numeric": 36,
      "code": "AU",
      "servers": {
        "pro": "https://au-api.neuron-mobility.com"
      },
      "cities": [
        {
          "name": "Brisbane",
          "code": "BNE",
          "latitude": -27.470192,
          "longitude": 153.025769,
          "zones": [
            {
              "code": "BB1",
              "cityCode": "BNE",
              "name": "Default",
              "baseFee": 1.0,
              "unitFee": 0.38,
              "createdAt": 1571017159000,
              "updatedAt": 1571017159000
            }
          ],
          ...
        },
        {
          "name": "Adelaide CBD",
          "code": "ADL-CBD",
          "latitude": -34.931056,
          "longitude": 138.603944,
          "zones": [
            {
              "code": "ADLCBD",
              "cityCode": "ADL-CBD",
              "name": "ADL-CBD",
              "baseFee": 1.0,
              "unitFee": 0.38,
              "createdAt": 1580262340000,
              "updatedAt": 1596159043000
            },
            {
              "code": "ADLNTH",
              "cityCode": "ADL-CBD",
              "name": "ADL-CBD",
              "baseFee": 1.0,
              "unitFee": 0.38,
              "createdAt": 1580262482000,
              "updatedAt": 1596159042000
            },
            {
              "code": "CHARLESSTURT",
              "cityCode": "ADL-CBD",
              "name": "CHARLESSTURT",
              "baseFee": 1.0,
              "unitFee": 0.38,
              "createdAt": 1605653824000,
              "updatedAt": 1605653824000
            },
            {
              "code": "NORWOOD",
              "cityCode": "ADL-CBD",
              "name": "NORWOOD",
              "baseFee": 1.0,
              "unitFee": 0.38,
              "createdAt": 1605653769000,
              "updatedAt": 1605653769000
            }
          ],
          ...
        },
        {
          "name": "Adelaide Western Alliance",
          "code": "ADL-WA",
          "latitude": -34.931056,
          "longitude": 138.540355,
          "zones": [
            {
              "code": "TEST1",
              "cityCode": "ADL-WA",
              "name": "TESTZONE",
              "baseFee": 1.0,
              "unitFee": 0.38,
              "createdAt": 1594338398000,
              "updatedAt": 1594345218000
            }
          ],
          ...
        },
        {
          "name": "Darwin",
          "code": "DRW",
          "latitude": -12.454054,
          "longitude": 130.846789,
          ...
        },
        {
          "name": "Canberra",
          "code": "CBR",
          "latitude": -35.28346,
          "longitude": 149.12807,
          ...
        },
        {
          "name": "Townsville",
          "code": "TSV",
          "latitude": -19.25097,
          "longitude": 146.81012,
          ...
        }
      ],
      ...
    },
    {
      "name": "New Zealand",
      "numeric": 554,
      "code": "NZ",
      "servers": {
        "pro": "https://nz-api.neuron-mobility.com"
      },
      "cities": [
        {
          "name": "Auckland",
          "code": "AKL",
          "latitude": -36.8485,
          "longitude": 174.7633,
          ...
        },
        {
          "name": "Dunedin",
          "code": "DUD",
          "latitude": -45.8666632,
          "longitude": 170.499998,
          ...
        }
      ],
      ...
    },
    {
      "name": "United Kingdom",
      "numeric": 826,
      "code": "GB",
      "digitalLines": [
        {
          "nickName": "Digital Line UK",
          "lang": "en"
        }
      ],
      "servers": {
        "pro": "https://gb-api.neuron-mobility.com"
      },
      "cities": [
        {
          "name": "Slough",
          "code": "SLG",
          "latitude": 51.50949,
          "longitude": -0.59541,
          ...
        },
        {
          "name": "Newcastle",
          "code": "NCL",
          "latitude": 54.97328,
          "longitude": -1.61396,
          ...
        }
      ],
      ...
    },
    {
      "name": "Korea",
      "numeric": 410,
      "code": "KR",
      "servers": {
        "pro": "https://kr-api.neuron-mobility.com"
      },
      "cities": [
        {
          "name": "Seoul",
          "code": "SEL",
          "latitude": 37.564539,
          "longitude": 126.980798,
          ...
        }
      ],
      ...
    }
  ]
}
```

</details>

To get the auth header (jwt), you have to login. First you have to aquire an password / login code that is sent to you by sms:
```
POST https://au-api.neuron-mobility.com/api/auth/sms

timestamp: 1616944256428
sign: 6cb9133f9d760f10d87541f060210aef

{"countryCode":"61","mobile":"401061840"}
```

After that, you can get the jwt auth header:
```
POST https://au-api.neuron-mobility.com/api/auth/v1/login

timestamp: 1616944354546
sign: 4133bccf633a388c127484da67e8692d

{"password": "763618","username": "401061840"}

{
	"data": {
		"token": "<jwt>",
		"refreshToken": "<token>",
		"isNewUser": true,
		"emailMarketing": true,
		"notificationMarketing": true,
		"locationMarketing": true,
		"usageTracking": true,
		"smsMarketing": true
	}
}
```

To get positions, put the jwt into the `X-Authorization` header:
```
GET https://au-api.neuron-mobility.com/api/scooters/userMap?distance=335&latitude=-27.469746172603777&longitude=153.02297267664835

X-Authorization: Bearer <jwt>
timestamp: 1616942614041
sign: 261492ea9bbf5bafca7ddc6dbba86c5d

{
	"data": [{
		"id": 5372,
		"qrCode": "51207883",
		"vehicleType": "SCOOTER",
		"helmetLock": true,
		"status": "IN_STATION",
		"latitude": -27.46785,
		"longitude": 153.02368,
		"remainingBattery": 0.46,
		"generation": "N3",
		"remainingRange": 13
	}, {
		"id": 6004,
		"qrCode": "51250199",
		"vehicleType": "SCOOTER",
		"helmetLock": true,
		"status": "IN_STATION",
		"latitude": -27.46757,
		"longitude": 153.02251,
		"remainingBattery": 0.67,
		"parkingTime": 1616934876000,
		"generation": "N3",
		"remainingRange": 32,
		"photoView": {
			"large": "https://images.neuron.sg/fit-in/800x/AU/SCOOTER/bf77e248d81145c4a548.JPEG",
			"small": "https://images.neuron.sg/fit-in/200x/AU/SCOOTER/bf77e248d81145c4a548.JPEG"
		}
	}
        //...
        ]
}
```

Credit [@robbi5](https://github.com/robbi5)
