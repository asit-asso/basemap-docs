# Integration

The vector tiles ecosystem mainly supports the Web Mercator coordinate system. Thus, it pretty straightforward to integrate our basemap in EPSG:3857 in all desktop- or web-based client. It's trickier to integrate our basemap in the swiss LV95 coordinate system (EPSG:2056) and a few client can do it :


| Client | EPSG:3857 | EPSG:2056 |
|---|---|---|
| MapLibre GL JS (web) | :lucide-check: | :lucide-x: |
| OpenLayers (web) | :lucide-check: | :lucide-check: |
| QGIS (desktop) | :lucide-check: | :lucide-x: |
| ArcGIS Pro (desktop) | :lucide-check: | :lucide-check: |

## Vector tiles basemap in LV95

In the LV95 basemap, the data are reprojected in LV95 before they are encoded as vector tiles and a custom tile grid is used to generate the tiles. 

Here are the main parameters of this tile grid. It's also available as an [OGC Two Dimensional Tile Matrix Set](../assets/LV95tilingSchemeASIT.json).

| Parameter | Value |
|---|---|
| Origin | `[2420000, 1350000]` |
| Tile Size | ``512`` |
| Resolutions | `65536, 32768, 16384, 8192, 4096, 2048, 1024, 512, 256, 128, 64, 32, 16, 8, 4, 2, 1, 0.5` |
| Zoom Levels | ``0-17`` |

In the LV95 tileset, the tile grid is also available directly in the `tiles.json` file under the `tileInfo` key :

```json
https://vt.asit-asso.ch/tiles/asit.cadastral/v1.0/2056/tiles.json

...
"tileInfo": {
	"rows": 512,
	"cols": 512,
	"dpi": 96,
	"format": "pbf",
	"origin": {
	"x": 2420000,
	"y": 1350000
	},
	"spatialReference": {
	"wkid": 2056,
	"latestWkid": 2056
	},
	"lods": [
		{
			"level": 0,
			"resolution": 65536,
			"scale": 247695118
		},
		{
			"level": 1,
			"resolution": 32768,
			"scale": 123847559
		},
		{
			"level": 2,
			"resolution": 16384,
			"scale": 61923780
		},
		{
			"level": 3,
			"resolution": 8192,
			"scale": 30961890
		},
		{
			"level": 4,
			"resolution": 4096,
			"scale": 15480945
		},
		...
	]
}
...

```