# Ofo

**Base url**: `https://one.ofo.com`

## Methods

### Login

Login is in two step: + Request code with your phone number, you ll receive an sms + Send back this code

#### Request sms code

**Method**: `POST`

**Path**: `/verifyCode_v2`

**Header**:

| Header       | Value                             | Mandatory |
| ------------ | --------------------------------- | :-------: |
| content-type | application/x-www-form-urlencoded |     X     |

**Parameters**:

| Parameters | Descriptions             | Mandatory |
| ---------- | ------------------------ | :-------: |
| tel        | phone number intl format |     X     |
| type       | Value at 1               |     X     |
| ccc        | Country calling codes (like `33`) |     X     |
| lat        | latitude                 |     X     |
| lng        | Longitude                |     X     |

**Exemple**

will send an sms to `06 12 34 56 78` phone with an OTP code.

```bash
curl --request POST \
  --url https://one.ofo.com/verifyCode_v2 \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'tel=612345678&type=1&ccc=33&lng=2.37&lat=48.85'
```

#### Send back OTP code

**Method**: `POST`

**Path**: `/api/login_v2`

**Header**:

| Header       | Value                             | Mandatory |
| ------------ | --------------------------------- | :-------: |
| content-type | application/x-www-form-urlencoded |     X     |

**Parameters**:

| Parameters | Descriptions             | Mandatory |
| ---------- | ------------------------ | :-------: |
| tel        | phone number intl format |     X     |
| code       | OTP code (length of 4)   |     X     |
| ccc        | Country calling codes (like `33`) |     X     |
| lat        | latitude                 |     X     |
| lng        | Longitude                |     X     |

**Exemple**

```bash
curl --request POST \
  --url https://one.ofo.com/api/login_v2 \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'tel=612345678&ccc=33&lng=2.37&lat=48.85&code=1234'
```

```JSON
{
	"errorCode": 200,
	"msg": "登陆成功",
	"values": {
		"token": "3655ff61-dc6c-4abb-a267-61c59b59eb73",
		"isNewuser": false,
		"needDeposit": false,
		"depositToPay": 0,
		"depositCurrency": "€",
		"paymentMethod": {}
	}
}
```

### Get bicycles by lat/lng

:warning: Auth are mandatory for this endpoint

**Method**: `POST`

**Path**: `/nearbyofoCar`

**Header**:

| Header       | Value                             | Mandatory |
| ------------ | --------------------------------- | :-------: |
| content-type | application/x-www-form-urlencoded |     X     |

**Parameters**:

| Parameters | Descriptions                  | Mandatory |
| ---------- | ----------------------------- | :-------: |
| lat        | latitude                      |     X     |
| lng        | Longitude                     |     X     |
| source     | ??? (maybe android or iphone) |     X     |
| token      | user token (get by auth)      |     X     |

**Exemple**

```bash
curl --request POST \
  --url https://one.ofo.so/nearbyofoCar \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data 'source=2&token=3655ff61-dc6c-4abb-a267-61c59b59eb73&lat=48.84&lng=2.38'
```

```JSON
{
	"errorCode": 200,
	"msg": "附近车辆位置",
	"values": {
		"cars": [
			{
				"carno": "PzeDlM",
				"bomNum": "5CA",
				"userIdLast": "1",
				"lng": 2.3796870561551,
				"lat": 48.839922829334
			}
    ]
  }
}
```

## Implementations

* https://www.npmjs.com/package/@multicycles/ofo (JavaScript)
* https://github.com/qxzzxq/pyofo (Python3)
* https://npmjs.org/ofo (JavaScript)
