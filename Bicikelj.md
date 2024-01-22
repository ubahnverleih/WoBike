# Bicikelj
[Bicikelj](https://www.bicikelj.si/en/) is an bike sharing provider.

### Get stations
```GET https://prominfo.projekti.si/web/api/MapService/Query/lay_bicikelj/query?f=json&returnGeometry=true&outSr=4326&geometry={"xmin":14.0321,"ymin":45.7881,"xmax":14.8499,"ymax":46.218,"spatialReference":{"wkid":4326}}&outFields=*```  
Add your location after `geometry`
