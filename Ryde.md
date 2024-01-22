# Ryde
[Ryde](https://www.ryde-technology.com) is an e-scooter sharing firm operating in Scandinavia.

To send requests to the API, no authentication is needed. However, you do need a cityId depending in which city you are. You can find an table at the end of this document.

### Get Scooters
```
POST https://qw-test.ryde.vip/appRyde/getNearScooters
Body:
iotLa: 63.4335711
iotLo: 10.3983865
nearRadius: 10
cityId: 5
```

`iotLa` and `iotLo` are your coordinates. Choose `cityId` from the list below.


| #   | City                      |
| --- | ------------------------- |
| 1   | Oslo                      |
| 5   | Trondheim                 |
| 6   | Kristiansand              |
| 7   | Stavanger                 |
| 11  | Karlstad                  |
| 12  | Halmstad                  |
| 15  | Bergen                    |
| 25  | Gävle                     |
| 26  | Tønsberg                  |
| 27  | Växjö                     |
| 28  | Havna(Lillestrøm)         |
| 29  | Stockholm                 |
| 30  | Umeå                      |
| 31  | Helsinki                  |
| 32  | Göteborg                  |
| 33  | Sundsvall                 |
| 34  | Turku                     |
| 35  | Tampere                   |
| 36  | Bodø                      |
| 37  | Örnsköldsvik              |
| 38  | Luleå                     |
| 39  | Jyväskylä                 |
| 40  | Pori                      |
| 41  | Oulu                      |
| 43  | Drammen                   |
| 44  | Skien                     |
| 45  | Sandefjord                |
| 46  | Fredrikstad & Sarpsborg   |
| 47  | Tromsø                    |
