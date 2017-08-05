# WoBike
Documentation of Bike Sharing APIs

It is userfull to show nearby bikesharing bikes in public transport apps. So i try to list the APIs from different bike sharing platforms. Also it could be interesting for multimodal routing apps.

## Nextbike (Worldwide)

URL: `https://api.nextbike.net/maps/nextbike-live.json` as JSON or `https://api.nextbike.net/maps/nextbike-live.xml` as XML. You also can filter by city, with the `GET`-Parameter `city`. Eg `https://api.nextbike.net/maps/nextbike-live.json?city=362` for Berlin.

For some citys nextbike has flexzones (free floating in this zones). At the moment these are:
 
 * Köln `https://api.nextbike.net/reservation/geojson/flexzone_kg.json`
 * Berlin `https://api.nextbike.net/reservation/geojson/flexzone_bn.json`
 * Dresden `https://api.nextbike.net/reservation/geojson/flexzone_sz.json`
 * Karlsruhe `https://api.nextbike.net/reservation/geojson/flexzone_fg.json`
 * Nürnberg `https://api.nextbike.net/reservation/geojson/flexzone_nb.json`

## Call-a-Bike (Germany / Deutsche Bahn)

Call-a-Bike has [historic datasets](http://data.deutschebahn.com/dataset/data-call-a-bike) OpenData on the DeutscheBahn OpenData portal under CC-BY License.

## oBike (Worldwide)

URL: `https://mobile.o.bike/api/v1/bike/list?latitude=<LAT>&longitude=<LON>` gives you all available bikes in a 1km*1km bounding box around the requested point. Example: `https://mobile.o.bike/api/v1/bike/list?latitude=48.1481&longitude=11.5755`

## ofo bike (China, UK, US)

tbd

## mobike (China, Italy, UK)

tbd

## yobike/ohbike

tbd

## Gobee bike (Hong Kong)

tbd

## bluegogo (China, US)

tbd

## LimeBike (US)

tbd

## Ford GoBike (US)

URL: `https://gbfs.fordgobike.com/gbfs/en/station_status.json`

## More...

Also have a look on [this Project](https://github.com/eskerda/pybikes/tree/master/pybikes)