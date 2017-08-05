# WoBike
Documentation of Bike Sharing APIs

It is userfull to show nearby bikesharing bikes in public transport apps. So i try to list the APIs from different bike sharing platforms. Also it could be interesting for multimodal routing apps.

## Nextbike (Worldwide)

URL: `https://api.nextbike.net/maps/nextbike-live.json` as JSON or `https://api.nextbike.net/maps/nextbike-live.xml` as XML. You also can filter by city, with the GET-Parameter `city`. Eg `https://api.nextbike.net/maps/nextbike-live.json?city=362` for Berlin.

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

For ofo bike you can only get the bike-locations if you have registered an account with a phone number. 

POST Request: `http://one.ofo.so/nearbyofoCar` with form parameters:

 * `lat`: `52.20`
 * `lng`: `0.134`
 * `source`: `1`
 * `token`: You need to register an account via phone number to get a token

## mobike (China, Italy, UK)

POST-Request to `https://mwx.mobike.com/mobike-api/rent/nearbyBikesInfo.do` with form parameters:

 * `latitude`: `22.5376`
 * `longitude`: `114.0577`

 Also you need to set the `Referer` header to `https://servicewechat.com/`.

 The requested radius looks very small.

## yobike/ohbike

POST-Request to `https://en.api.ohbike.com/v1/vehicle/` with form-paramters:
 
 * `bounds`: `51.319838,-2.735466;51.605178,-2.477974` (boundingbox)
 * `coord_type`: `1`
 * `distance`: `700`
 * `ak`: `WWTaJQrg-NHe_Zl0iwghHyYypYw6g-6GEZHPGBBF6TI7OzZWo9VVLXWRs2ngQJ18` (Application Key)
 * `lat`: `51.462731`
 * `lng`: `-2.606720`
 * `zoom`: `11.000000`
 * `sign`: `58f66c2424cb6d410e9277a8f6cc81b4` md5 sum

The sign parameter is a md5 sum of all paramters. You need to build it, by:
 * get all POST *values* (excl. sign)
 * sort the values
 * concat the sorted values (ignoring empty values) to string
 * add `@ohbile` on the end of the string
 * md5 sum the string and add as sign paramter
 * string of this example would be `-2.606720111.00000051.319838,-2.735466;51.605178,-2.47797451.462731700WWTaJQrg-NHe_Zl0iwghHyYypYw6g-6GEZHPGBBF6TI7OzZWo9VVLXWRs2ngQJ18geonear@ohbile` and the md5: `58f66c2424cb6d410e9277a8f6cc81b4`

## Gobee bike (Hong Kong)

tbd

## bluegogo (China, US)

tbd

## LimeBike (US)

tbd

## Ford GoBike (US)

URL: `https://gbfs.fordgobike.com/gbfs/en/station_status.json`

## More...

Also have a look at [this Project](https://github.com/eskerda/pybikes/tree/master/pybikes)