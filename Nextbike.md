# Nextbike (Worldwide)

## API

URL: `https://api.nextbike.net/maps/nextbike-live.json` as JSON or `https://api.nextbike.net/maps/nextbike-live.xml` as XML.

### Filtering

You also can filter by city, with the GET-Parameter `city`. Eg. `https://api.nextbike.net/maps/nextbike-live.json?city=362` for Berlin.
In some cities there's an artificial split into separate systems (while the bikes are interchangeable) - you can provide multiple city ids separated by commas - e.g. `https://nextbike.net/maps/nextbike-official.xml?city=372,210,475` lists all Warsaw bikes. For a list of valid city values, see the section `Cities` below

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


## Cities
Currently Nextbike operates in the following regions:
|Country Code|City Name|Bikeshare Name|city id|
|----|----|----|---|
|AT|Innsbruck|Stadtrad Innsbruck Austria|199|
|AT|Klagenfurt|nextbike Klagenfurt Austria|396|
|AT|Kufstein|VVT REGIORAD Tirol|773|
|AT|Linz|city bike Linz|692|
|AT|Neusiedler See|nextbike Burgenland Austria|23|
|AT|Serfaus|nextbike Tirol Austria|286|
|AT|St.Pölten|nextbike Niederösterreich Austria|57|
|AT|Wien|WienMobil Rad|748|
|BA|Banja Luka|BL bike (BIH)|494|
|BA|Sarajevo|nextbike BIH|350|
|BA|Zenica|Zenica (BIH)|427|
|CH|Sursee Plus|nextbike Switzerland|88|
|CY|Limassol|nextbike Cyprus|190|
|CZ|Benešov|nextbike Benešov|887|
|CZ|Berounsko|nextbike Berounsko|655|
|CZ|Brno|nextbike Brno|660|
|CZ|Česká Třebová|nextbike Česká Třebová|869|
|CZ|Dvůr Králové|nextbike Dvůr Králové|768|
|CZ|Frýdek-Místek|nextbike Frýdek-Místek|700|
|CZ|Havířov|nextbike Havířov|637|
|CZ|Hořice|nextbike Hořice|888|
|CZ|Hradec Králové|nextbike Hradec Králové|682|
|CZ|Jablonec nad Nisou|nextbike Jablonec|876|
|CZ|Jihlava|nextbike Jihlava|719|
|CZ|Kladno|nextbike Kladno|659|
|CZ|Klášterec nad Ohří|nextbike Klášterec nad Ohří|866|
|CZ|Krnov|nextbike Krnov|707|
|CZ|Mladá Boleslav|nextbike Mladoboleslavsko|681|
|CZ|Moravská Třebová|nextbike Moravská Třebová|865|
|CZ|Olomouc|nextbike Olomouc|663|
|CZ|Opava|nextbike Opava|662|
|CZ|Ostrava|nextbike Ostrava|271|
|CZ|Pardubice|nextbike Pardubice|680|
|CZ|Písek|nextbike Písek|709|
|CZ|Praha|nextbike Praha|661|
|CZ|Přerov|nextbike Přerov|917|
|CZ|Prostejov|nextbike Prostejov|549|
|CZ|Rychnovsko|nextbike Rychnovsko|708|
|CZ|Třebíč|nextbike Třebíč|863|
|CZ|Uherské Hradiště|nextbike Uherské Hradiště|702|
|CZ|Vrchlabí|nextbike Vrchlabí|769|
|CZ|Zlín|nextbike Zlín|703|
|DE|Augsburg|SWA Rad|178|
|DE|Bad Oeynhausen|Oeynrad|689|
|DE|Bergisches e-Bike|Bergisches e-Bike|676|
|DE|Berlin|nextbike Berlin|362|
|DE|Bielefeld|meinSiggi|16|
|DE|Bonn|Bonn nextbike|547|
|DE|Braunschweig|Nibelungen-Bike|657|
|DE|Bremen|WK-Bike (Bremen)|379|
|DE|Dortmund|metropolradruhr Germany|129|
|DE|Dresden|MOBIbike|685|
|DE|Düsseldorf|nextbike Düsseldorf|50|
|DE|Eifel e-Bike|Eifel e-Bike|706|
|DE|Erfurt|nextbike Erfurt|493|
|DE|Erkelenz|westBike|807|
|DE|Erlangen|nextbike Erlangen|829|
|DE|Frankfurt|nextbike Frankfurt|8|
|DE|Freiburg|Frelo Freiburg|619|
|DE|Gießen|nextbike Gießen|467|
|DE|Graben|Graben - ready4green|647|
|DE|Grünheide (Mark)|EDEKA Grünheide|819|
|DE|Gütersloh|nextbike Gütersloh|160|
|DE|Hanau|Heraeus Hanau|301|
|DE|Hannover|sprintRAD|87|
|DE|Heidelberg|VRNnextbike|194|
|DE|Karlsruhe|KVV.nextbike|21|
|DE|Kassel|nextbike Kassel|462|
|DE|Köln|KVB Rad Germany|14|
|DE|Lahr|nextbike Lahr (Pedelecs)|505|
|DE|Leipzig|nextbike Leipzig|1|
|DE|Leverkusen|wupsiRad Leverkusen|607|
|DE|Lippstadt|nextbike Lippstadt|536|
|DE|Marburg|nextbike Marburg|438|
|DE|Mönchengladbach|NEW MöBus nextbike|530|
|DE|Norderstedt|nextbike Norderstedt|177|
|DE|Nürnberg|VAG_Rad|626|
|DE|Offenburg|nextbike Offenburg|155|
|DE|Oldenburg|OLi-Bike|754|
|DE|Potsdam|Potsdam Rad|158|
|DE|REVG|mobic|817|
|DE|Roche - Mannheim|DCS Bike|860|
|DE|RSVG|RSVG-Bike|691|
|DE|Rüsselsheim am Main|nextbike Rüsselsheim am Main|439|
|DE|RVK e-Bike|RVK|648|
|DE|SAP Walldorf|SAP Walldorf|592|
|DE|Schkeuditz|Landkreis Nordsachsen - Deutschland|867|
|DE|Tecklenburg|Tecklenburger Land|858|
|DE|Wiesbaden|nextbike Wiesbaden|7|
|DE|Winsen (Luhe)|WinsenRad|794|
|ES|Barcelona|Ambici |833|
|ES|Bilbaobizi|Bilbaobizi (Bilbao)|532|
|ES|Getxobizi|Getxobizi|749|
|ES|Ibiza-City|ibizi|629|
|ES|Las Palmas de Gran Canaria|Sitycleta (Las Palmas)|408|
|ES|León|nextbike León|701|
|ES|Lovesharing (Canary Islands) - Gran Canaria|Lovesharing (Canary Islands)|638|
|ES|Mislata|BiciMislata Mislata|835|
|ES|Palma|BiciPalma|789|
|ES|Urdaibai|BBK Klimabizi|772|
|GB|Belfast|BelfastBikes|238|
|GB|Brunel University|Santander Cycles - Brunel|487|
|GB|Cardiff|OVO Bikes Cardiff & Vale of Glamorgan|476|
|GB|Exeter|Co-bikes|354|
|GB|Glasgow|OVO Bikes Glasgow|237|
|GB|Milton Keynes|Santander Cycles - Milton Keynes|320|
|GB|Stirling|nextbike Stirling|243|
|GB|Swansea University|Santander Cycles - Swansea|486|
|HR|Brinje|Općina Brinje (Croatia)|325|
|HR|Drniš|Grad Drniš (Croatia)|426|
|HR|Ivanic Grad|Grad Ivanić-Grad (Croatia)|337|
|HR|Jastrebarsko|Jastrebarsko (Croatia)|425|
|HR|Karlovac|Grad Karlovac (Croatia)|305|
|HR|Križevci|Grad Križevci (Croatia)|714|
|HR|Makarska|Grad Makarska (Croatia)|324|
|HR|Općina Dugopolje|Grad Split (Croatia)|445|
|HR|Osijek|eMobi (Croatia)|734|
|HR|Pitomača|Općina Pitomača (Croatia)|574|
|HR|Poreč|Porec bike share (Croatia)|429|
|HR|Pula|Grad Pula (Croatia)|798|
|HR|Šibenik|Grad Šibenik (Croatia)|248|
|HR|Sisak|Grad Sisak (Croatia)|416|
|HR|Slavonski Brod|Grad Slavonski Brod (Croatia)|308|
|HR|Stari Grad |Hvar (Croatia)|745|
|HR|Velika Gorica|Grad Velika Gorica (Croatia)|415|
|HR|Vinkovci|Grad Vinkovci (Croatia)|739|
|HR|Vukovar|Grad Vukovar (Croatia)|328|
|HR|Zadar|Grad Zadar (Croatia)|327|
|HR|Zagreb|nextbike Croatia|220|
|HR|Zaprešić|Grad Zaprešić (Croatia)|723|
|IE|Limerick|Castletroy Bikes (Ireland)|862|
|IT|Bergamo|nextbike Bergamo|728|
|IT|Gorizia|Nomago Bikes - GO2GO Gorizia|771|
|IT|Scandicci|ARVAL|746|
|LV|Rīga|nextbike LV|128|
|MX|San Luis Potosi|YOY - San Luis Potosi|664|
|PL|Białystok|BIKER Białystok Poland|245|
|PL|Chorzów|Kajteroz - Chorzowski Rower Miejski Poland|557|
|PL|Ciechanów|Ciechanowski Rower Miejski Poland|523|
|PL|Częstochowa|Częstochowski Rower Miejski Poland|450|
|PL|Grodzisk Mazowiecki|GRM Grodzisk Poland|255|
|PL|GZM - Test city|GZM - Poland|872|
|PL|Jastrzębie-Zdrój|JasKółka|509|
|PL|Katowice|Katowice Bike Poland|342|
|PL|Kędzierzyn-Koźle|OK Bike Poland|421|
|PL|Kołobrzeg|Kołobrzeski Rower Miejski Poland|422|
|PL|Koluszki (RL)|Rowerowe Łódzkie Poland (RL)|562|
|PL|Konin|Koniński Rower Miejski Poland|545|
|PL|Koszalin|Koszaliński Rower Miejski Poland|496|
|PL|Łomża|ŁoKeR - Łomża|796|
|PL|Olesno|Oleski Rower Miejski Poland|650|
|PL|Ostrów Wielkopolski|Rower Miejski w Ostrowie Wielkopolskim Poland|452|
|PL|Otwock|Otwocki Rower Miejski Poland|527|
|PL|Piaseczno|Piaseczyński Rower Miejski Poland|461|
|PL|Pielgrzymka|System Roweru Gminnego Poland|593|
|PL|Piotrków Trybunalski|Piotrkowski Rower Miejski Poland|518|
|PL|Pobiedziska|Pobiedziski Rower Gminny Poland|504|
|PL|Pruszków|Pruszkowski Rower Miejski Poland|442|
|PL|Pszczyna|System Rowerów Miejskich w Pszczynie Poland|411|
|PL|Sokołów Podlaski|Rower Powiatowy Sokołów Podlaski|727|
|PL|Sosnowiec|Sosnowiecki Rower Miejski Poland|497|
|PL|Szamotuły|Rower Miejski Szamotuły Poland (RMS) Poland|446|
|PL|Tarnów|Tarnowski Rower Miejski Poland|548|
|PL|Tychy|Tyski Rower Miejski Poland|413|
|PL|Warszawa|VETURILO 3.0|812|
|PL|Wolsztyn|Wolsztyński Rower Miejski|831|
|PL|Wrocław|WRM nextbike Poland|148|
|PL|Zielona Góra|Zielonogórski Rower Miejski Poland|529|
|PL|Żyrardów|Żyrardowski Rower Miejski Poland|551|
|RO|Campia Turzii|BikeCity Campia Turzii|750|
|RO|Drobeta|Drobeta Velopark|693|
|RO|Focșani|nextbike Romania|510|
|RO|Topoloveni|Topoloveni Bike|832|
|SE|Göteborg|Styr & Ställ (Sweden, Göteborg)|658|
|SI|Celje|Nomago Bikes - KOLESCE|531|
|SI|Kranjska Gora|Nomago Bikes - KRANJSKA GORA|780|
|SI|Ljubljana|Nomago Bikes -  LJUBLJANA|717|
|SI|Nova Gorica|Nomago Bikes - GO2GO|688|
|SI|Portorož|Nomago Bikes - PORTOROZ|806|
|SI|Zagorje ob Savi|Nomago Bikes - ZANAPREJ|725|
|SK|Senec|Arriva Nitra Slovakia|875|
|SK|Žilina|BikeKIA|538|
|UA|Lviv|nextbike (Ukraine)|280|
|UA|Vinnytsia|nextbike Vinnitsa (Ukraine)|546|

This table can be generated by:
```sh
curl https://api.nextbike.net/maps/nextbike-live.json | jq '.countries[] | "|\(.country)|\(.cities[0].name)|\(.name)|\(.cities[0].uid)|"' | tr -d '"' | grep -v null | sort
```
