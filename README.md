# WoBike

Documentation of Bike Sharing APIs

Public transport and multimodal routing apps could benefit from showing nearby bikes from bikesharing services. So here's a list showing the APIs of a few of these platforms.

## Nextbike (Worldwide)

URL: `https://api.nextbike.net/maps/nextbike-live.json` as JSON or `https://api.nextbike.net/maps/nextbike-live.xml` as XML. You also can filter by city, with the GET-Parameter `city`. Eg `https://api.nextbike.net/maps/nextbike-live.json?city=362` for Berlin.

For some cities nextbike has flexzones (free floating in these zones). At the moment these are:

* Köln `https://api.nextbike.net/reservation/geojson/flexzone_kg.json`
* Berlin `https://api.nextbike.net/reservation/geojson/flexzone_bn.json`
* Dresden `https://api.nextbike.net/reservation/geojson/flexzone_sz.json`
* Karlsruhe `https://api.nextbike.net/reservation/geojson/flexzone_fg.json`
* Nürnberg `https://api.nextbike.net/reservation/geojson/flexzone_nb.json`
* For all zones: `https://api.nextbike.net/reservation/geojson/flexzone_all.json`

## Call-a-Bike (Germany / Deutsche Bahn)

Call-a-Bike has [historic datasets](http://data.deutschebahn.com/dataset/data-call-a-bike) OpenData on the DeutscheBahn OpenData portal under CC-BY License.

You can use the [Flinkster API](http://data.deutschebahn.com/dataset/flinkster-api) with `providernetwork=2` to access Call-a-Bike live Data. You need to register on [developer.deutschebahn.com](https://developer.deutschebahn.com/store/site/pages/sign-up.jag) to get a free, unlimited API Key (Zugangstoken).

Example Request: `https://api.deutschebahn.com/flinkster-api-ng/v1/bookingproposals?lat=48.15&lon=11.5&radius=5000&limit=100&providernetwork=2` – You also have to set the `Authorization` header to `Bearer <YOUR-API-KEY>`

* Paramter `limit` max value is `100`, but you can use `offset` to request more
* Paramter `radius` is the searchradius in meters, max value is `10000`, min value is `100`, default `500`,
* You can also add parameter `expand` to `rentalobject,price` to get vehicle and price info

There is also a [Documentation PDF](https://developer.deutschebahn.com/store/site/themes/responsive/templates/api/documentation/download.jag?tenant=carbon.super&resourceUrl=/registry/resource/_system/governance/apimgt/applicationdata/provider/DBOpenData/Flinkster_API_NG/v1/documentation/files/Schnittstellenspezifikation_FlinksterApiNG.pdf) (german only), and you can use the [API-console (3rd tab)](https://developer.deutschebahn.com/store/apis/info?name=Flinkster_API_NG&version=v1&provider=DBOpenData)

## oBike (Worldwide)

URL: `https://mobile.o.bike/api/v1/bike/list?latitude=<LAT>&longitude=<LON>` gives you all available bikes in a 1km\*1km bounding box around the requested point. Example: `https://mobile.o.bike/api/v1/bike/list?latitude=48.1481&longitude=11.5755`

## ofo bike (China, UK, US, Austria, Thailand, Singapore, France)

[Detailed documentation](Ofo.md)

## mobike (China, Italy, UK, Japan)

POST-Request to `https://mwx.mobike.com/mobike-api/rent/nearbyBikesInfo.do` with form parameters:

* `latitude`: `22.5376`
* `longitude`: `114.0577`

Also you need to set the `Referer` header to `https://servicewechat.com/`.
Maybe you need to set `user-agent` header to `MicroMessenger/6.5.4.1000 NetType/WIFI Language/zh_CN`

The requested radius looks very small.

## yobike/ohbike (UK)

[Detailed documentation](Yobike.md)

## Gobee bike (Hong Kong, France, Belgium, Italy)

Simple GET-Request example: `http://aws.gobee.bike/GobeeBike/bikes/near_bikes?accuracy=20&lat=22.38&lng=114.198`

## bluegogo (China, US)

POST-Request to `https://api-us.bluegogo.com/nearbyBikes?data={"token":"","version":1,"data":{"latitude":22.5526,"longitude":114.1029,"billingModelIds":"1,2,3"}}`

(The JSON should be URLencoded.) – If the token is empty, the bikelist will also be empty. I guess you get a token when signing in.

## LimeBike (US)

Simple GET-Request example: `https://web-production.lime.bike/api/public/v1/views/bikes?map_center_latitude=37.7604&map_center_longitude=-122.4398&user_latitude=37.7604&user_longitude=-122.4398`

Returns 50 nearest bikes.

## Motivate (US)

[Motivate](https://www.motivateco.com/) builds Bikesharing Systems. They publish their [Data and APIs](https://www.motivateco.com/use-our-data/). This includes the following systems (cities) in the US:

* Ford GoBike (Bay Area, CA)
  * GBFS: `https://gbfs.fordgobike.com/gbfs/gbfs.json`
* Biketown (Portland, OR)
* Capital Bikeshare (Washington, DC)
* Bike Chattanooga (Chattanooga, TN)
* Citi Bike (New York)
* CoGo (Columbus, OH)
* Divvy (Chicago, IL)
* Hubway (Boston, MA)

All APIs and data are also listed on `https://www.motivateco.com/use-our-data/`

Motivate also has undocumented APIs in the GeoJSON format. URLs might be tricky to divine for other cities, but Washington DC & NYC are:
 * https://layer.bicyclesharing.net/map/v1/wdc/map-inventory
 * https://layer.bicyclesharing.net/map/v1/nyc/map-inventory

## BYKE (Germany)

Simple GET-Request example: `https://api-prod.ibyke.io/v1/bikes?latitude=52.55001&longitude=13.40902&order=nearby`

## dropbike (Canada)

* `POST`-Request: `https://dropbikeadminapi.herokuapp.com/v1/bikes_nearby`
* (Header `Content-Type` to `application/json`)
* Request Payload example: `{"lat":43.659415191015498,"lng":-79.395512826740742}`

* You can also get their regions with a simple `POST`-Request (without payload) to `https://dropbikeadminapi.herokuapp.com/v1/region_polygons`

## JUMP (USA)

[JUMP](http://jumpbikes.com/) operates electric dockless bikeshares in Washington, DC & San
Francisco. They operate open data APIs at https://dc.jumpmobility.com/opendata
and https://sf.jumpmobility.com/opendata respectively.

## SocialBicycles (USA, Canada, Czech Republic, Poland)

[SocialBicycles](http://socialbicycles.com/) is JUMP's partership-based bikeshare program. They publish their [Data and APIs](https://app.socialbicycles.com/developer/#!/networks). This includes the following systems (cities) around the world:

* SoBi Hamilton (Hamilton, ON, CA)
* Velonet (Czech Republic)
* Wavelo (Warsaw, Poland)
* many more..

## More...

* Also have a look at [this project](https://github.com/eskerda/pybikes/tree/master/pybikes)
* [GBFS (General Bikeshare Feed Specification)](https://github.com/NABSA/gbfs)
* [Bikesharing World Map](https://www.google.com/maps/d/u/0/viewer?mid=1UxYw9YrwT_R3SGsktJU3D-2GpMU&ll=50.01042750703113%2C35.03132237929685&z=2)
* [Open Bike Share Data](https://bikeshare-research.org/)

## Todo

* [Spin](https://www.spin.pm/) (uses certificate pinning)
* [Pony Bikes](http://getapony.com/) (uses certificate pinning)
* [Donkey Republic](http://www.donkey.bike/) (uses certificate pinning)
