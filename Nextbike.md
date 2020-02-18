# Nextbike (Worldwide)

## API

URL: `https://api.nextbike.net/maps/nextbike-live.json` as JSON or `https://api.nextbike.net/maps/nextbike-live.xml` as XML.

### Filtering

You also can filter by city, with the GET-Parameter `city`. Eg. `https://api.nextbike.net/maps/nextbike-live.json?city=362` for Berlin.
In some cities there's an artificial split into separate systems (while the bikes are interchangeable) - you can provide multiple city ids separated by commas - e.g. `https://nextbike.net/maps/nextbike-official.xml?city=372,210,475` lists all Warsaw bikes.

### Flexzones

For some cities nextbike has flexzones (free floating in these zones). At the moment these are:

* Berlin `https://api.nextbike.net/reservation/geojson/flexzone_bn.json`
* Dresden `https://api.nextbike.net/reservation/geojson/flexzone_sz.json`
* Karlsruhe `https://api.nextbike.net/reservation/geojson/flexzone_fg.json`
* Köln `https://api.nextbike.net/reservation/geojson/flexzone_kg.json`
* Leipzig `https://api.nextbike.net/reservation/geojson/flexzone_le.json`
* Nürnberg `https://api.nextbike.net/reservation/geojson/flexzone_nb.json`
* For all zones: `https://api.nextbike.net/reservation/geojson/flexzone_all.json`

## GBFS

Nextbike provides different GBFS endpoints for their different networks/brands/countries:
`https://api.nextbike.net/maps/gbfs/v1/nextbike_<NETWORK-ID>/gbfs.json`. You can find the Network-ID in the `https://api.nextbike.net/maps/nextbike-live.json`, just search for the `"domain"` JSON Key in country root object. So GBFS endpoint for Germany would be `https://api.nextbike.net/maps/gbfs/v1/nextbike_de/gbfs.json`. Warning: Not all german citys are in the `de` endpoint, e.g. in Berlin nextbike partnered with Deezer and uses Network-ID/country code `bn`.
