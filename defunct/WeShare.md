# WeShare (defunct)

[WeShare](https://www.we-share.io/) was a carsharing company based in Germany providing vehicles in Berlin and Hamburg.

There is *no* authentication or special headers and only `GET`-Requests required for their API-Endpoints.

## Request available Cities (and their ids)

You need to know the id of your city first, so send a GET request to `https://mos-p-euw-weshare-apm.azure-api.net/assets/getCities`

The result looks like this:

```json
{
  "data": [
    {
      "cityId": "81cb439f-3b59-11e9-9928-06cf929d95ea",
      "name": "Berlin",
      "cityName": "Berlin",
      "countryName": "Deutschland",
      "midpoint": {
        "lat": 52.52437,
        "lon": 13.41053
      },
      "radius": 6000,
      "supportPhoneNumber": "+4930255585657",
      "helpUrl": "https://www.we-share.io/faq-in-app/",
      "imprintUrl": "https://www.we-share.io/imprint-in-app/",
      "serviceDocuments": {
        "serviceDocumentsId": "146fa38f-d300-4ce6-94e8-64189a063353",
        "termsAndConditionsUrl": "https://www.we-share.io/app-terms-in-app/",
        "dataPrivacyPolicyUrl": "https://www.we-share.io/app-privacy-in-app/",
        "version": 2
      }
    },
    {
      "cityId": "75d659f4-7978-42ca-b2ee-b25efcc5eaa9",
      "name": "Hamburg",
      "cityName": "Hamburg",
      "countryName": "Deutschland",
      "midpoint": {
        "lat": 53.552546,
        "lon": 10.014038
      },
      "radius": 6000,
      "supportPhoneNumber": "+4930255585657",
      "helpUrl": "https://www.we-share.io/faq-in-app",
      "imprintUrl": "https://www.we-share.io/imprint-in-app/",
      "serviceDocuments": {
        "serviceDocumentsId": "146fa38f-d300-4ce6-94e8-64189a063353",
        "termsAndConditionsUrl": "https://www.we-share.io/app-terms-in-app/",
        "dataPrivacyPolicyUrl": "https://www.we-share.io/app-privacy-in-app/",
        "version": 2
      }
    }
  ]
}
```

## Get all available vehicles

Now you can load all available vehicles using another GET request to `https://mos-p-euw-weshare-apm.azure-api.net/assets/getAvailableVehicles?cityId=<YOUR_CITY_ID>`

The result contains an array with all vehicles in the following format:

```json
{
  "data": [
    {
      "id": "007f5ae4-6902-9f85-0294-3e8f7c8a937d",
      "plate": "WI-D5320E",
      "iconBaseUrl": "https://storage.googleapis.com/weshare-public-assets/vehicle-images/ID3/529",
      "cityId": "81cb439f-3b59-11e9-9928-06cf929d95ea",
      "serviceId": "ccf9d6e2-d945-46d5-96f6-5f77464a4f15",
      "modelName": "ID.3",
      "updatedAt": null,
      "pricingModels": {
        "standard": {
          "currency": "EUR",
          "basicFee": 1,
          "perMinute": 0.29,
          "perMinuteStopOver": 0.29,
          "perDay": 58,
          "mileage": {
            "included": 100,
            "perExtraUnit": 0.29
          }
        },
        "wesharePlus": {
          "currency": "EUR",
          "basicFee": 1,
          "perMinute": 0.19,
          "perMinuteStopOver": 0.05,
          "perDay": 48,
          "mileage": {
            "included": 150,
            "perExtraUnit": 0.29
          }
        }
      },
      "location": {
        "lat": 52.53689,
        "lon": 13.26205
      },
      "autonomyLevel": 30,
      "autonomyRange": 101,
      "isCharging": false
    }
  ]
}
```

## Get the served area

This GET request returns all limits of the service-area: `https://mos-p-euw-weshare-apm.azure-api.net/assets/getHomeZone?cityId=<YOUR_CITY_ID>`

Result similar to:

```json
{
"data": [
    {
      "zoneId": "1035edc5c05445129e609b1012f90a8e",
      "name": "Lidl - Prinzenallee 75",
      "area": [
        {
          "latitude": 52.555029,
          "longitude": 13.383484
        },
        {
          "latitude": 52.555248,
          "longitude": 13.382687
        },
        {
          "latitude": 52.55487,
          "longitude": 13.382435
        },
        {
          "latitude": 52.554656,
          "longitude": 13.38321
        },
        {
          "latitude": 52.555029,
          "longitude": 13.383484
        }
      ],
      "forbidden": true
    }
  ]
}
```
