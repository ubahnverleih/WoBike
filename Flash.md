# Flash / Circ (scooters)

# Request scooters

Send a `GET` request to `https://api.goflash.com/api/Mobile/Scooters?userLatitude=47.36&userLongitude=8.55&lang=de&latitude=47.36&longitude=8.55&latitudeDelta=0.01&longitudeDelta=0.01`

## Parameters

| Name  | Function |
| ------------- | ------------- |
| userLatitude / userLongitude | Location of the "user", used to determine how far they are from the scooter. You can set it to whatever you want.  |
| lang | Seems to only influence the `txtRentalPrice`, not very important.  |
| latitude / longitude | Location of where you want to search for scooters  |
| latitudeDelta / longitudeDelta | Boundaries of the search zone, note that you can't set it to something very big, it seems to max out at some point.  |

## Response

You can get a sample response (even with only one scooter, it's a verbose API...) below.

The interessting bits are in `Data.Scooters` for the scooters, and in `Data.City` where you can get the current city info / boundaries.

````
{
  "Result": "OK",
  "ResponseText": null,
  "Data": {
    "TermsConditions": {
      "AcceptedTermsConditions": true,
      "UrlPrivacy": "",
      "UrlTerms": ""
    },
    "Scooters": [
      {
        "idScooter": 20820,
        "idCity": "LYS",
        "ScooterCode": "129765",
        "idScooterState": "DEPLOYED_FOR_RENTAL",
        "LatLng": {
          "_geometry": {
            "m_points": [
              {
                "x": 45.76449850000001,
                "y": 4.877939
              }
            ],
            "m_zValues": null,
            "m_mValues": null,
            "m_figures": [
              {
                "pointOffset": 0,
                "figureAttribute": "Stroke"
              }
            ],
            "m_shapes": [
              {
                "figureOffset": 0,
                "parentOffset": -1,
                "type": "Point"
              }
            ],
            "m_fValid": true,
            "m_extendedUserProperties": "None"
          },
          "_isNull": false,
          "_srid": 4326
        },
        "Distance": 142.97,
        "PowerPercent": "39%",
        "RemainderRange": "18 km",
        "PowerPercentInt": 39,
        "ScooterModel": "Flash N1",
        "IsBookable": false,
        "IsTootable": false,
        "StreetInfo": "Rue du 4 Août 1789 35",
        "StreetInfo2": "69100 Villeurbanne",
        "txtRentalPrice": "1€ zum Entsperren + 0.15€ pro Minute",
        "BookingDurationMinutes": 5,
        "ShowRoute": true,
        "Distance_txt": "142m",
        "Locked": true,
        "location": {
          "latitude": 45.76449850000001,
          "longitude": 4.8779390000000005
        },
        "GoogleMapsMode": "bicycling",
        "GoogleMapsEstimatedTimeRatio": 1,
        "CostEstimationEnabled": true,
        "CityName": "Lyon",
        "ScooterStatus": "DEPLOYED_FOR_RENTAL",
        "GSMCoverage": 27,
        "SatelliteNumber": 12,
        "GPSStatus_txt": "53s",
        "GPSStatus_sort": "2019-05-03 01:44:22_129765",
        "GPSStatus_int": 3,
        "IOTStatus_txt": "51s",
        "IOTStatus_sort": "2019-05-03 01:44:22_129765",
        "IOTStatus_int": 3,
        "txt_Code": "129765",
        "sort_Code": "129765",
        "txt_Model": "Flash N1",
        "sort_Model": "Flash N1",
        "txt_Location": "Rue du 4 Août 1789 35, 69100 Villeurbanne",
        "sort_Location": 142.97,
        "txt_Power": "39%",
        "sort_Power": 39,
        "txt_GSM": "51s",
        "sort_GSM": "2019-05-03 01:44:22_129765",
        "txt_GPS": "53s",
        "sort_GPS": "2019-05-03 01:44:22_129765",
        "txt_Status": "DEPLOYED_FOR_RENTAL",
        "sort_Status": "DEPLOYED_FOR_RENTAL"
      }
    ],
    "City": {
      "idCity": "LYS",
      "RideablePolygon": "POLYGON ((4.8712752 45.7856705, 4.8613264 45.7875427, 4.8587515 45.7878271, 4.8566058 45.7879618, 4.8521426 45.788261, 4.8497394 45.7888819, 4.847379 45.7896225, 4.8414996 45.7908943, 4.8362031 45.7918695, 4.8278346 45.7934405, 4.8264184 45.7941437, 4.8271909 45.7946224, 4.8302808 45.7961783, 4.8379626 45.7983326, 4.8432841 45.8088634, 4.8322978 45.8081455, 4.8254314 45.8052736, 4.813356 45.7976136, 4.8088928 45.7925569, 4.8043438 45.7958184, 4.796147 45.7914198, 4.7924133 45.7948609, 4.7914905 45.7916292, 4.788508 45.7882179, 4.7860402 45.7855095, 4.7825428 45.7820529, 4.7812124 45.7804666, 4.7809978 45.7788205, 4.7818561 45.7777729, 4.7860618 45.7742409, 4.7900959 45.7710081, 4.7849248 45.7657331, 4.7815774 45.7605836, 4.78132 45.7553738, 4.7762559 45.7550744, 4.7685312 45.7520201, 4.7759126 45.7469891, 4.7833799 45.7434552, 4.8006748 45.7500437, 4.8075305 45.7538542, 4.813605 45.7504268, 4.8159868 45.7500974, 4.8190552 45.7513102, 4.8202999 45.7502777, 4.8158367 45.7472231, 4.813948 45.7442091, 4.8130039 45.738818, 4.8155357 45.732049, 4.8217851 45.7203878, 4.8233193 45.7200132, 4.8253149 45.7180956, 4.826096 45.7175827, 4.8325011 45.7216276, 4.8351404 45.7250505, 4.8356554 45.7255449, 4.8359987 45.726556, 4.8377951 45.7264508, 4.8389616 45.7263682, 4.8396736 45.7264169, 4.840171 45.7262257, 4.8409066 45.7258109, 4.841443 45.725354, 4.8420975 45.7247698, 4.842473 45.7239759, 4.8427505 45.7186495, 4.8430401 45.717833, 4.8434601 45.7170547, 4.8440272 45.7165784, 4.8445637 45.7164585, 4.8454756 45.7164884, 4.85614 45.7174923, 4.8778801 45.719643, 4.891098 45.7237776, 4.8929614 45.7259263, 4.8981756 45.7299107, 4.8985618 45.7304274, 4.8999386 45.7328847, 4.9007229 45.7342093, 4.9039845 45.7386573, 4.904316 45.7400735, 4.906146 45.7461424, 4.9058819 45.7476304, 4.906311 45.7505651, 4.9083286 45.7581107, 4.9087149 45.7604161, 4.9088435 45.7614042, 4.9087148 45.7624221, 4.9078565 45.7637095, 4.9061614 45.7655656, 4.9043374 45.7671224, 4.894522 45.7791555, 4.8927302 45.7810111, 4.8911638 45.7831211, 4.8891897 45.785231, 4.8877413 45.7866525, 4.8872049 45.787049, 4.8855446 45.7874811, 4.8849625 45.7875671, 4.8837287 45.7877279, 4.8825673 45.7877616, 4.8814515 45.7877298, 4.880274 45.7875783, 4.8787854 45.7872566, 4.8756526 45.7865833, 4.8712752 45.7856705))",
      "LatitudeCityCenter": 45.756148,
      "LongitudeCityCenter": 4.840322,
      "RedPolygonSizeKm": 40,
      "cityOverlayRedColor": "rgba(255, 155, 155, 0.2)",
      "cityOverlayRedBorderColor": "rgba(255, 155, 155, 0.4)",
      "cityOverlayGreenColor": "rgba(128, 255, 128, 0.2)",
      "cityOverlayGreenBorderColor": "rgba(128, 255, 128, 0.4)",
      "SpecialAreas": []
    },
    "Stripe": {
      "UserAddedPayment": false,
      "StripeApiKey": null,
      "AndroidPayMode": null
    },
    "OpsFilternScooters": 0,
    "PreRideURL": ""
  }
}
````
