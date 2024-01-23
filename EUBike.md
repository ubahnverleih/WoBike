# Introduction (defunct)
EU-BIKE was a Swedish bike sharing service available in some bigger and smaller Swedish cities.
### Viewing bikes

**PLEASE NOTE: EUBIKE has had some serious problems related to displaying correct locations of bikes. I know that because I have tried their system and I have read some reviews online. With that said, not all bike locations are inaccurate.**

To see the list of available bikes for a location, you don´t need any authentication. The app "EU-BIKE" is connecting to an Alibaba Cloud server
at the IP "47.91.87.181" at port 8080.
This is a basic URL to list available bikes:
http://47.91.87.181:8080/UserApi/AppUser?lng=18.0668918788433075&lat=59.3109666242335791&cmd=areabike&dist=0.5 (**POST**)
The app adds a "logintoken" to the request, but it is not required in order to see bikes.
Here is a detailed explaination of each accepted key in the request:
**lng**: The longtitude of the search.
**lat**: The latitude of the search.
**cmd**: I am unsure what this means, butit it is probably to identify the query request.
**dist**: The distance (from the latitude and longtitude parameters) to perform a seach from. Probably in a radius format and in kilometers. Doesn´t seem to have an upper limit. The app´s default value when performing a search is 0.5.

Example response (in JSON, with a distance value of 0.5):
`{"success":true,"data":[{"devid":"10002165","shebeistatus":0,"devlat":59.31163,"devlng":18.06885,"devpower":0,"dist":0.133420259},{"devid":"10000991","shebeistatus":0,"devlat":59.30986,"devlng":18.06548,"devpower":0,"dist":0.147037461},{"devid":"10001231","shebeistatus":0,"devlat":59.3098221,"devlng":18.0654163,"devpower":0,"dist":0.15217796},{"devid":"10001958","shebeistatus":0,"devlat":59.3130035,"devlng":18.0675964,"devpower":0,"dist":0.230080515},{"devid":"10001357","shebeistatus":0,"devlat":59.31213,"devlng":18.0620575,"devpower":0,"dist":0.303918362},{"devid":"10002477","shebeistatus":0,"devlat":59.3123932,"devlng":18.0622749,"devpower":0,"dist":0.306723356},{"devid":"10001918","shebeistatus":0,"devlat":59.3137474,"devlng":18.0648251,"devpower":0,"dist":0.330999374},{"devid":"10000956","shebeistatus":0,"devlat":59.3138161,"devlng":18.0649929,"devpower":0,"dist":0.334925145},{"devid":"10002714","shebeistatus":0,"devlat":59.3076477,"devlng":18.0688934,"devpower":0,"dist":0.386613458},{"devid":"10001682","shebeistatus":0,"devlat":59.3134842,"devlng":18.0621243,"devpower":0,"dist":0.390006721},{"devid":"10000901","shebeistatus":0,"devlat":59.30748,"devlng":18.0644283,"devpower":0,"dist":0.412274241},{"devid":"10002153","shebeistatus":0,"devlat":59.3124542,"devlng":18.07411,"devpower":0,"dist":0.442282349},{"devid":"10002307","shebeistatus":0,"devlat":59.3143,"devlng":18.0712128,"devpower":0,"dist":0.444873065},{"devid":"10001967","shebeistatus":0,"devlat":59.3112335,"devlng":18.0747318,"devpower":0,"dist":0.446369052},{"devid":"10002368","shebeistatus":0,"devlat":59.310463,"devlng":18.0749969,"devpower":0,"dist":0.463875979},{"devid":"10002375","shebeistatus":0,"devlat":59.31275,"devlng":18.0743713,"devpower":0,"dist":0.469272941},{"devid":"10003093","shebeistatus":0,"devlat":59.3102531,"devlng":18.0750942,"devpower":0,"dist":0.472640872},{"devid":"10001165","shebeistatus":1,"devlat":59.3100624,"devlng":18.0752888,"devpower":0,"dist":0.487477541},{"devid":"10001402","shebeistatus":0,"devlat":59.3102951,"devlng":18.0754051,"devpower":0,"dist":0.489349037}],"maparea":[]}`

Explaination of values returned in the response:

**success**: Pretty self-explainatory. Indicates if a request has failed or not.

**Please note: If there is no bikes available at the performed query, the server will return False as the success value and "Interface call error" in a "msg" parameter.**

### Bike object (listed in the "data" list returned on a successfull request):

**devid** : The device ID of the bike. This ID is probably used when unlocking a bike, but I have not confirmed that.

**sheibeistatus** : Unconfirmed. Seems to always be set to zero.

**devlat** : The latitude position of the bike.

**devlng** : The longtitude position of the bike.

**devpower** : Probably has to do with the device power, but it seems to always be set to zero, even with bikes that have power.

**dist** : The distance to the bike from the coordinates given in the request. This value is most likely in kilometers.

[END OF BIKE OBJECT]

**maparea** : Not confirmed.
