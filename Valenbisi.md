# Valenbisi

## Introduction
Valenbisi is a station bases bike share system in Valencia, Spain. It is operated and maintained by JCDecaux.

## API
The API supplies data about stations and docked bikes every 10 minutes released under a CC BY 4.0 license. Authentication is not required.

[More information](https://valencia.opendatasoft.com/explore/dataset/valenbisi-disponibilitat-valenbisi-dsiponibilidad/information/)

**Base URL**: `https://valencia.opendatasoft.com/api/explore/v2.1/catalog/datasets/valenbisi-disponibilitat-valenbisi-dsiponibilidad/records`

**Data Schema:**
| name | type | description | sample |
| :--- | :--- | :---| :--- |
| address | text | Street/address of the station | `Navarro Reverter - Grabador Esteve` |
| number | integer | House number where the station is located | `28` |
| open | text | Station status. T=open, F=closed | `T` |
| available | integer | Number of available bikes | `15` |
| free | integer | Numebr of free docking slots | `14` |
| total | integer | Total number of docking slots | `29` |
| ticket | text | unknown | `F` |
| updated_at | text | Date of the last update of the station data | `01/03/2026 14:29:46` |
| geo_shape | geo shape | Coordinates of the station as point | `{"coordinates":[-0.36775135880068294,39.47158231958138],"type":"Point"}` |
| geo_point_2d | geo point | Coordinates of the station as x,y | [39.47158231958138,-0.36775135880068294] |
| update_jcd | datetime | unknown | `2026-03-01T15:28:55+01:00` |


**Example results**
```{
    "total_count":273,
    "results":[
        {
            "address":"Juan Llorens - Quart",
            "number":19,
            "open":"T",
            "available":1,
            "free":14,
            "total":15,
            "ticket":"F",
            "updated_at":"01/03/2026 14:49:06",
            "geo_shape":{
                "type":"Feature",
                "geometry":{
                    "coordinates":[
                        -0.391590438798602,
                        39.47436934054566
                    ]
                    ,
                    "type":"Point"
                }
                ,
                "properties":{
                }
            }
            ,
            "geo_point_2d":{
                "lon":-0.391590438798602,
                "lat":39.47436934054566
            }
            ,
            "update_jcd":"2026-03-01T14:39:04+00:00"
        }
        ,
        […]
    ]
}
```