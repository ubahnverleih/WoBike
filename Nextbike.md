# Nextbike (Worldwide)

## API

URL: `https://maps.nextbike.net/maps/nextbike-live.json` as JSON or `https://maps.nextbike.net/maps/nextbike-live.xml` as XML.

### Filtering

You also can filter by city, with the GET-Parameter `city`. Eg. `https://maps.nextbike.net/maps/nextbike-live.json?city=362` for Berlin.
In some cities there's an artificial split into separate systems (while the bikes are interchangeable) - you can provide multiple city ids separated by commas - e.g. `https://maps.nextbike.net/maps/nextbike-official.xml?city=372,210,475` lists all Warsaw bikes. For a list of valid city values, see the section `Cities` below

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
`https://gbfs.nextbike.net/maps/gbfs/v2/nextbike_<NETWORK-ID>/gbfs.json`. You can find the Network-ID in the `https://maps.nextbike.net/maps/nextbike-live.json?list_cities=1`, just search for the `"domain"` JSON Key in country root object. So GBFS endpoint for Germany would be `https://gbfs.nextbike.net/maps/gbfs/v2/nextbike_de/gbfs.json`. Warning: Not all german citys are in the `de` endpoint, e.g. in Berlin nextbike partnered with Deezer and uses Network-ID/country code `bn`.


## Cities
Currently Nextbike operates in the following regions:
|Country Code|City Name|Bikeshare Name|city id|
|----|----|----|---|
|AT|Innsbruck|Stadtrad Innsbruck Austria|199|
|AT|Klagenfurt|nextbike Klagenfurt Austria|396|
|AT|Kufstein|VVT REGIORAD Tirol|773|
|AT|Linz|city bike Linz|692|
|AT|St. Pölten|nextbike Niederösterreich|57|
|AT|Wien|WienMobil Rad|748|
|BA|Banja Luka|BL bike|494|
|BA|Sarajevo|nextbike BIH|350|
|BA|Zenica|Zenica|427|
|BE|Affligem|nextbike BE Vlaamse Rand|1099|
|CH|Chur|MOOINZ|1055|
|CH|Sursee Plus|nextbike Switzerland|88|
|CY|Limassol|nextbike Cyprus|190|
|CZ|Benešov|nextbike Benešov|887|
|CZ|Berounsko|nextbike Berounsko|655|
|CZ|Brno|nextbike Brno|660|
|CZ|Dvůr Králové|nextbike Dvůr Králové|768|
|CZ|Frýdek-Místek|nextbike Frýdek-Místek|700|
|CZ|Hodonín|nextbike Hodonín|1046|
|CZ|Hořice|nextbike Hořice|888|
|CZ|Hradec Králové|nextbike Hradec Králové|682|
|CZ|Jablonec nad Nisou|nextbike Jablonec|876|
|CZ|Jičín|nextbike Jičín|1032|
|CZ|Kladno|nextbike Kladno|659|
|CZ|Klášterec nad Ohří|nextbike Klášterec nad Ohří|866|
|CZ|Kolín|nextbike Kolín|1137|
|CZ|Liberec|nextbike Liberec|793|
|CZ|Mladá Boleslav|nextbike Mladoboleslavsko|681|
|CZ|Moravská Třebová|nextbike Moravská Třebová|865|
|CZ|Most|nextbike Most|1036|
|CZ|OlbramoviceVotice|nextbike OlbramoviceVotice|1034|
|CZ|Olomouc|nextbike Olomouc|663|
|CZ|Opava|nextbike Opava|662|
|CZ|Ostrava|nextbike Ostrava|271|
|CZ|Pelhřimov|nextbike Pelhřimov|919|
|CZ|Praha|nextbike Praha|661|
|CZ|Písek|nextbike Písek|709|
|CZ|Přerov|nextbike Přerov|917|
|CZ|Rychnovsko|nextbike Rychnovsko|708|
|CZ|Trutnov|nextbike Trutnov|792|
|CZ|Třebíč|nextbike Třebíč|863|
|CZ|Uherské Hradiště|nextbike Uherské Hradiště|702|
|CZ|Vrchlabí|nextbike Vrchlabí|769|
|CZ|Zlín|nextbike Zlín|703|
|CZ|Česká Třebová|nextbike Česká Třebová|869|
|DE|Bad Neuenahr-Ahrweiler|AW-bike|968|
|DE|Bergisches e-Bike|Bergisches e-Bike|676|
|DE|Berlin|nextbike Berlin|362|
|DE|Bielefeld|meinSiggi|16|
|DE|Bonn|Bonn nextbike|547|
|DE|Braunschweig|Veloleo|657|
|DE|Bremen|Bre.Bike|379|
|DE|Dortmund|metropolradruhr|129|
|DE|Dresden|MOBIbike|685|
|DE|Düsseldorf|nextbike Düsseldorf|50|
|DE|Eifel e-Bike|Eifel e-Bike|706|
|DE|Erkelenz|westBike|807|
|DE|Frankfurt|nextbike Frankfurt|8|
|DE|Freiburg|Frelo|619|
|DE|Friedrichsdorf|flux Bikesharing|954|
|DE|Gießen|nextbike Gießen|467|
|DE|Greifswald (StadtRad)|StadtRad Greifswald|759|
|DE|Grünheide (Mark)|EDEKA Grünheide|819|
|DE|Gütersloh|nextbike Gütersloh|160|
|DE|Halle (Saale)|movemix_bike|943|
|DE|Hanau|Heraeus Hanau|301|
|DE|Hannover|nextbike Hannover|87|
|DE|Heidelberg|VRNnextbike|194|
|DE|Karlsruhe|KVV.nextbike|21|
|DE|Kassel|nextbike Kassel|462|
|DE|Konstanz|Mein konrad|1042|
|DE|Köln|KVB Rad|14|
|DE|Leipzig|nextbike Leipzig|1|
|DE|Leverkusen|wupsiRad Leverkusen|607|
|DE|Lippstadt|HelBi (Kreis Soest)|536|
|DE|MV-Rad|MV-Rad|775|
|DE|Marburg|nextbike Marburg|438|
|DE|Mönchengladbach|NEW MöBus nextbike|530|
|DE|Norderstedt|nextbike Norderstedt|177|
|DE|Nürnberg|VAG_Rad|626|
|DE|Offenburg|EinfachMobil|155|
|DE|Oldenburg|OLi-Bike|754|
|DE|Potsdam|Potsdam Rad|158|
|DE|REVG|mobic|817|
|DE|RSVG|RSVG-Bike|691|
|DE|RVK e-Bike|RVK|648|
|DE|Roche - Mannheim|Roche - Bike|860|
|DE|Rüsselsheim am Main|nextbike Rüsselsheim am Main|439|
|DE|SAP Walldorf|SAP Walldorf|592|
|DE|Schkeuditz|Landkreis Nordsachsen|867|
|DE|Tecklenburg|Tecklenburger Land|858|
|DE|Usedom |UsedomRad|176|
|DE|Wiesbaden|nextbike Wiesbaden|7|
|DE|Winsen (Luhe)|WinsenRad|794|
|ES|AMB|AMBici|833|
|ES|Arteixo|biciArteixo|874|
|ES|Bilbaobizi|Bilbaobizi (Bilbao)|532|
|ES|Bilbao|bizkaibizi (Spain)|873|
|ES|Boadilla del Monte|bibo|1134|
|ES|Las Palmas de Gran Canaria|moxsi (Las Palmas de Gran Canaria)|408|
|ES|León|nextbike León|701|
|ES|Logroño|nextbike BiciLOG|1052|
|ES|Lovesharing (Canary Islands) - Gran Canaria|Lovesharing (Canary Islands)|638|
|ES|Mislata|BiciMislata|835|
|ES|Ontinyent|Ontibici|951|
|ES|Palma|BiciPalma|789|
|ES|Santander|TUeBICI|914|
|ES|Torrent|TORRENTBICI|1056|
|ES|Urdaibai|BBK Klimabizi|772|
|FI|Helsinki|Helsinki (Demo)|1139|
|FR|Bischheim|Vélhop - Strasbourg|924|
|FR|Mulhouse|VéloCité Mulhouse|1124|
|GB|Belfast|BelfastBikes|238|
|GB|Bristol|WESTcargo powered by nextbike|946|
|GB|Brunel University|Santander Cycles - Brunel|487|
|GB|Glasgow|nextbike Glasgow|237|
|GB|Milton Keynes|Santander Cycles - Milton Keynes|320|
|GB|Stirling|nextbike Stirling|243|
|GB|Swansea University|Swansea University Cycles|486|
|GR|Alimos|Bike Sharing Alimos|1029|
|GR|Elliniko|Bike Sharing Elliniko – Argyroupoli|1030|
|HR|Camp Lanterna|BIKE SHARE LANTERNA(Croatia)|1086|
|HR|Drniš|Grad Drniš (Croatia)|426|
|HR|Ivanic Grad|Grad Ivanić-Grad (Croatia)|337|
|HR|Jastrebarsko|Jastrebarsko (Croatia)|425|
|HR|Karlovac|Grad Karlovac (Croatia)|305|
|HR|Križevci|Grad Križevci|714|
|HR|Općina Brinje|Općina Brinje (Croatia)|325|
|HR|Općina Dugopolje|Grad Split|445|
|HR|Općina Stankovci|Dalmacija (Croatia)|977|
|HR|Osijek|eMobi|734|
|HR|Poreč|Porec bike share (Croatia)|429|
|HR|Sisak|Grad Sisak (Croatia)|416|
|HR|Slavonski Brod|Grad Slavonski Brod (Croatia)|308|
|HR|Stari Grad|Hvar (Croatia)|745|
|HR|Velika Gorica|Grad Velika Gorica (Croatia)|415|
|HR|Vinkovci|Grad Vinkovci (Croatia)|739|
|HR|Vukovar|Grad Vukovar (Croatia)|328|
|HR|Zadar|Grad Zadar (Croatia)|327|
|HR|Zagreb|nextbike Croatia|220|
|HR|Zaprešić|Grad Zaprešić (Croatia)|723|
|HR|Šibenik|Grad Šibenik (Croatia)|248|
|IT|Bergamo|nextbike Bergamo|728|
|IT|Senigallia|Fa.Mo.Se|839|
|IT|nextCity|nextbike the world|1084|
|LV|Rīga|nextbike LV|128|
|PL|Białystok|BIKER Białystok|245|
|PL|Chełm|Chełmski Rower|776|
|PL|Częstochowa|Częstochowski Rower Miejski Poland|450|
|PL|Grodzisk Mazowiecki|GRM Grodzisk Poland|255|
|PL|Józefów|Veloyou - Józefowski Rower Miejski|1051|
|PL|Koluszki (RL)|Rowerowe Łódzkie|562|
|PL|Konin|Koniński Rower Miejski Poland|545|
|PL|Koszalin|Koszaliński Rower Miejski Poland|496|
|PL|Kołobrzeg|Kołobrzeski Rower Nextbike|422|
|PL|Kędzierzyn-Koźle|OK Bike|421|
|PL|MultisportBike|MultiSportBike|1141|
|PL|Nextbike Polska|Nextbike Polska|732|
|PL|Ostrów Wielkopolski|Rower Miejski w Ostrowie Wielkopolskim|452|
|PL|Otwock|Otwocki Rower Miejski|527|
|PL|Piaseczno|Piaseczyński Rower Miejski|461|
|PL|Pobiedziska|Pobiedziski Rower Gminny|504|
|PL|Pruszków|Pruszkowski Rower Miejski|442|
|PL|Pszczyna|System Rowerów Miejskich w Pszczynie|411|
|PL|Tarnów|Tarnowski Rower Miejski|548|
|PL|Tychowo|Tychowski Rower Miejski|528|
|PL|Tychy (GZM)|METROROWER|958|
|PL|Warszawa|VETURILO 3.0|812|
|PL|Wolsztyn|Wolsztyński Rower Miejski|831|
|PL|Wrocław|WRM nextbike Poland|148|
|PL|Łomża|ŁoKeR - Łomża|796|
|PL|Łódź - Łódzki Rower Publiczny|Łódzki Rower Publiczny|330|
|PL|Żyrardów|Żyrardowski Rower Miejski|551|
|PT|Barcelos|TubaBike (Barcelos)|916|
|RO|Alba Iulia|Alba Iulia Velocity|950|
|RO|Buzău|Buzau VeloCity|800|
|RO|Campia Turzii|BikeCity Campia Turzii|750|
|RO|Focșani|nextbike Romania|510|
|RO|Slatina|Slatina Velo City|945|
|RO|Slobozia|Slobozia Bike City|978|
|RO|Topoloveni|Topoloveni Bike|832|
|SE|Gothenborg Cargo|Gothenburg Cargo|1090|
|SE|Göteborg|Styr & Ställ (Sweden, Göteborg)|658|
|SI|Celje|Nomago Bikes - KOLESCE|531|
|SI|Gorizia|Nomago Bikes - GO2GO Gorizia|771|
|SI|Kranjska Gora|Nomago Bikes - KRANJSKA GORA|780|
|SI|Ljubljana|Nomago Bikes -  LJUBLJANA in MEDVODE|717|
|SI|Nova Gorica|Nomago Bikes - GO2GO|688|
|SI|Portorož|Nomago Bikes - PORTOROZ|806|
|SI|Radlje ob Dravi|Nomago Bikes - SLOVENIA|922|
|SI|Zagorje ob Savi|Nomago Bikes - ZANAPREJ|725|
|SK|Senec|Arriva Bike|875|
|SK|Senica|Senica bajk|942|
|SK|Žilina|BikeKIA|538|
|XK|Prishtina|Prishtina bike|291|

This table can be generated by:
```sh
curl "https://maps.nextbike.net/maps/nextbike-live.json?list_cities=1" | jq '.countries[] | "|\(.country)|\(.cities[0].name)|\(.name)|\(.cities[0].uid)|"' | tr -d '"' | grep -v null | sort
```
