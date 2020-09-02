# Wegue configuration

This describes the Wegue application configuration, which is modelled as JSON document.

## Properties

### General

| Property          | Mandatory | Meaning | Example |
|-------------------|:---------:|---------|---------|
| title             |  | Title shown in the top toolbar | `"title": "A Wegue WebGIS App"` |
| baseColor         |  | Main colour of the UI elements | `"baseColor": "red darken-3"` or `"baseColor": "#ff3388"` |
| logo              |  | URL to an image shown as application logo | ` "logo": "https://dummyimage.com/100x100/aaa/fff&text=Wegue"`
| logoSize          |  | Size of the application logo defined in `logo` | `"logoSize": "100"` |
| footerTextLeft    |  | Text or HTML string to be displayed in the left side of the toolbar | `"footerTextLeft": "Powered by <a href='https://meggsimum.de/wegue/' target='_blank'>Wegue WebGIS</a>"` |
| footerTextRight   |  | Text or HTML string to be displayed in the right side of the toolbar | `"footerTextRight": "meggsimum"` |
| showCopyrightYear |  | Boolean value, whether the copyright year should be shown on the right side of the toolbar | `"showCopyrightYear": true` or `"showCopyrightYear": false` | 
| mapZoom           | x | Initial zoom level of the map | `"mapZoom": 2` |
| mapCenter         | x | Initial center of the map in map projection | `"mapCenter": [0, 0]` |
| mapProjection     |   | Configuration object for CRS / projection used for the map | see [mapProjection](wegue-configuration?id=mapprojection) |
| modules           | x | Array of module configuration objects | See [modules](module-configuration) |
| mapLayers         | x | Array of map layer configuration objects | See [mapLayers](map-layer-configuration) |
| projectionDefs    |   | Array of CRS / projection definition objects compatible to proj4js | See [projectionDefs](wegue-configuration?id=projectiondefs) |
| tileGridDefs      |   | Array of tile grid definition objects | See [tileGridDefs](wegue-configuration?id=tilegriddefs) |

### projectionDefs

The property `projectionDefs` is a nested array holding several [proj4js](https://proj4js.org) compatible projection definitions. For each array element the first item is the projection code and the second item is the [proj4](https://proj4.org) definition string. For example:

```json
[
    "EPSG:28992",
    "+proj=sterea +lat_0=52.15616055555555 +lon_0=5.38763888888889 +k=0.999908 +x_0=155000 +y_0=463000 +ellps=bessel +units=m +towgs84=565.2369,50.0087,465.658,-0.406857330322398,0.350732676542563,-1.8703473836068,4.0812 +no_defs"
]
```
Projection definitions can be found at [epsg.io](http://epsg.io). The definition in the example above would therefore be available at http://epsg.io/28992.

### mapProjection

The property `mapProjection` defines the CRS, which is used by the map in your Wegue application. The `mapProjection` object has the following properties:

| Property          | Mandatory | Meaning | Example |
|-------------------|-----------|---------|---------|
| code              | x | The code of the SRS to be used for the map. In case it is not `EPSG:4326` or `EPSG:3857` it has to be defined in the [projectionDefs](wegue-configuration?id=projectiondefs) | `"code": "EPSG:28992"` |
| units             | x | The unit of the SRS | `"units": "m"` |
| extent            | x | The validity extent for the SRS | `"extent": [-285401.920, 22598.080, 595401.920, 903401.920]` |

### tileGridDefs

By default Wegue (OpenLayers) assumes the "OSM/Google" Web Mercator tilegrid. Different tilegrids can be defined once and then referenced in Layer configurations.
Below is an example for the Dutch Standard Tilegrid:

```json
"tileGridDefs": {
    "dutch_rd": {
      "extent": [-285401.920, 22598.080, 595401.920, 903401.920],
      "resolutions": [3440.640, 1720.320, 860.160, 430.080, 215.040, 107.520, 53.760, 26.880, 13.440, 6.720, 3.360, 1.680, 0.840, 0.420, 0.210],
      "tileSize": [256, 256]
    }
  }
 ```
 One or more tilegrids can be defined. The tilegrid's name is arbitrary, above `dutch_rd`, and can be used as a key when refered in in a Layer configuration.
 Each tilegrid definition has the following properties:
 
| Property          | Mandatory | Meaning | Example |
|-------------------|-----------|---------|---------|
| extent            | x | The extent of the tilegrid as a bounding box using the SRS of the Map| `"extent": [-285401.920, 22598.080, 595401.920, 903401.920]` |
| resolutions       | x | The resolutions for the tilegrid (related to zoomlevels) as an array | `"resolutions": [3440.640, 1720.320, 860.160, 430.080, 215.040, 107.520]` |
| tileSize          | x | The tilesize in width/height pixels | `"tileSize": [512, 512]` |

In a Layer configuration a specific tilegrid can be refered to as follows, using the `tileGridRef` property:

```
    {
      "type": "XYZ",
      "lid": "brtachtergrondkaart",
      "name": "WMTS - Topo Basemap - PDOK",
      "url": "https://geodata.nationaalgeoregister.nl/tiles/service/wmts/brtachtergrondkaart/EPSG:28992/{z}/{x}/{y}.png",
      "projection": "EPSG:28992",
      "tileGridRef": "dutch_rd",
      "visible": true
    }
```

## Example configuration

Example configurations can be found in the `app-starter/static` directory. Below an example as used in the Demo:

```json
{

  "title": "Vue.js / OpenLayers WebGIS",

  "baseColor": "red darken-3",

  "logo": "https://dummyimage.com/100x100/aaa/fff&text=Wegue",
  "logoSize": "100",

  "footerTextLeft": "Powered by <a href='https://meggsimum.de/wegue/' target='_blank'>Wegue WebGIS</a>",
  "footerTextRight": "meggsimum",
  "showCopyrightYear": true,

  "mapZoom": 2,
  "mapCenter": [0, 0],

  "mapLayers": [

    {
      "type": "VECTOR",
      "lid": "Shops",
      "name": "Shops DaSchau",
      "url": "./static/data/shops-dannstadt.geojson",
      "formatConfig": {
      },
      "format": "GeoJSON",
      "visible": true,
      "selectable": true,
      "style": {
        "radius": 4,
        "strokeColor": "purple",
        "strokeWidth": 2,
        "fillColor": "rgba(155,153,51,0.5)"
      }
    },
    {
      "type": "VECTOR",
      "lid": "earthquakes",
      "name": "Earthquakes 2012 (Mag 5)",
      "url": "./static/data/2012_Earthquakes_Mag5.kml",
      "formatConfig": {
        "extractStyles": false
      },
      "format": "KML",
      "visible": true,
      "attributions": "U.S. Geological Survey",
      "selectable": true,
      "hoverable": true,
      "hoverAttribute": "name",
      "style": {
        "iconUrl": "./static/icon/circle.svg",
        "scale": 4,
        "anchor": [0.5, 37],
        "anchorXUnits": "fraction",
        "anchorYUnits": "pixels"
      }
    },
    {
      "type": "WMS",
      "lid": "ahocevar-wms",
      "name": "WMS (ahocevar)",
      "format": "image/png",
      "layers": "topp:states",
      "url": "https://ahocevar.com/geoserver/wms",
      "transparent": true,
      "singleTile": false,
      "projection": "EPSG:3857",
      "attribution": "",
      "isBaseLayer": false,
      "visible": false,
      "displayInLayerList": true
    },

    {
      "type": "VECTORTILE",
      "name": "Vector Tile Layer",
      "url": "https://ahocevar.com/geoserver/gwc/service/tms/1.0.0/ne:ne_10m_admin_0_countries@EPSG%3A900913@pbf/{z}/{x}/{-y}.pbf",
      "format": "MVT",
      "visible": false,
      "style": {
        "strokeColor": "gray",
        "strokeWidth": 1,
        "fillColor": "rgba(20,20,20,0.1)"
      }
    },

    {
      "type": "XYZ",
      "name": "OpenTopoMap",
      "url": "https://tile.opentopomap.org/{z}/{x}/{y}.png",
      "attributions": "Map data: <a href=\"https://openstreetmap.org/copyright\">©OpenStreetMap</a>-contributors, SRTM | Map representation (Kartendarstellung): © <a href=\"http://opentopomap.org/\">OpenTopoMap</a> (<a href=\"https://creativecommons.org/licenses/by-sa/3.0/\">CC-BY-SA</a>)",
      "lid": "opentopomap",
      "displayInLayerList": true,
      "visible": false
    },

    {
      "type": "OSM",
      "lid": "osm-bg",
      "name": "OSM",
      "isBaseLayer": false,
      "visible": true,
      "displayInLayerList": true
    }

  ],

  "modules": {
    "wgu-layerlist": {
      "target": "menu",
      "win": true,
      "draggable": false
    },
    "wgu-measuretool": {
      "target": "menu",
      "win": true,
      "draggable": false,
      "strokeColor": "#c62828",
      "fillColor": "rgba(198,40,40,0.2)",
      "sketchStrokeColor": "rgba(198,40,40,0.8)",
      "sketchFillColor": "rgba(198,40,40,0.1)",
      "sketchVertexStrokeColor": "#c62828",
      "sketchVertexFillColor": "rgba(198,40,40,0.2)"
    },
    "wgu-infoclick": {
      "target": "menu",
      "win": true,
      "draggable": false,
      "initPos": {
        "left": 8,
        "top": 74
      }
    },
    "wgu-geocoder": {
      "target": "toolbar",
      "darkLayout": true,
      "minChars": 2,
      "queryDelay": 200,
      "selectZoom": 16,
      "debug": false,
      "placeHolder": "Search address",
      "provider": "osm",
      "providerOptions": {
        "lang": "en-US",
        "countrycodes": "",
        "limit": 6
      }
    },
    "wgu-zoomtomaxextent": {
      "target": "toolbar",
      "darkLayout": true
    },
    "wgu-helpwin": {
      "target": "toolbar",
      "darkLayout": true
    }
  }
}
```

A more elaborate example, named [app-conf-projected.json](https://github.com/meggsimum/wegue/blob/master/app-starter/static/app-conf-projected.json) with custom Projections and Tilegrids can be found in the app-starter directory.

