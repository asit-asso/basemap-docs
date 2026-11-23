# Web

## MapLibre GL JS
MapLibre is one of the most performant client to render vector tiles basemap but does not work with the swiss LV95 coordinate system.

After importing the necessary dependencies and creating a `map` container, you simply need to point to a `style.json` file :


```html
<head>
	...
	<link href="https://unpkg.com/maplibre-gl@^6.10.0/dist/maplibre-gl.css" rel="stylesheet" />
</head>

<body>
	<div id="map" style="height: 100vh;"></div>
	<script type="module">

		import * as maplibregl from 'https://unpkg.com/maplibre-gl@^6.10.0/dist/maplibre-gl.mjs';

		// Initialize map with the first style
		const map = new maplibregl.Map({
			container: "map",  // container id
			style: "https://vt.asit-asso.ch/styles/3857/asit.color/style.json",  // style.json URL for the color basemap in epsg:3857
			center: [8.23, 46.8],  // starting position in WGS84 [lng, lat]
			zoom: 7,  // starting zoom (in Web Mercator tile grid)
		});
	</script>
</body>
```

## OpenLayers
With the help of the [ol-mapbox-style](https://openlayers.org/ol-mapbox-style/) plugin, OpenLayers can also render vector tiles by simply reading a `style.json` file.

First import the necessary OpenLayers and ol-mapbox-style dependencies :

```html
<head>
	...
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ol@10.10.0/ol.css" />
	<script src="https://cdn.jsdelivr.net/npm/ol@10.10.0/dist/ol.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/ol-mapbox-style@12.4/dist/olms.js"></script>
</head>
```

Then initiate a map container :

```html
<body>
	<div id="map" style="height: 100vh;"></div>
</body>
```

After creating an OpenLayers map instance to get the right view, you can apply a ``style.json`` with the `apply` function of the `olms` plugin. Since, by default, OpenLayers render vector tiles partially as raster layer, we set the `pixelRatio` to `2` in the map instance to improve render sharpness between zoom level. Tuning this parameter is a tradeoff between render sharpness and performance. See [OpenLayers VectorTileLayer renderMode parameter](https://openlayers.org/en/latest/apidoc/module-ol_layer_VectorTile.html#~VectorTileRenderType).

```html
<body>
	<div id="map" style="height: 100vh;"></div>
	<script>

		const map = new ol.Map({
			target: 'map',  // container id
			pixelRatio: 2,
			view: new ol.View({
				center: ol.proj.fromLonLat([8.23, 46.8]),  // starting position in WGS84 [lng, lat]
				zoom: 7  // starting zoom (in Web Mercator tile grid)
			}),
		});

		olms.apply(
			map,  // OpenLayers map instance
			"https://vt.asit-asso.ch/styles/3857/asit.color/style.json"  // style.json URL for the color basemap in epsg:3857
		)

	</script>
</body>
```

## OpenLayers in LV95

OpenLayers can also integrate vector tiles in any custom coordinate system, e.g. the swiss LV95.

First, import the necessary dependencies and initiate a map container :

```html
<head>
	...
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ol@10.10.0/ol.css" />
	<script src="https://cdn.jsdelivr.net/npm/ol@10.10.0/dist/ol.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/ol-mapbox-style@12.4/dist/olms.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/proj4@2.11.0/dist/proj4.min.js"></script>
</head>

<body>
	<div id="map" style="height: 100vh;"></div>
</body>
```

Then, reference the EPSG:2056 coordinate system :

```html
<script>

	proj4.defs(
		'EPSG:2056',
		'+proj=somerc +lat_0=46.9524055555556 +lon_0=7.43958333333333 ' +
		'+k_0=1 +x_0=2600000 +y_0=1200000 +ellps=bessel ' +
		'+towgs84=674.374,15.056,405.346,0,0,0,0 +units=m +no_defs +type=crs'
	);
	ol.proj.proj4.register(proj4);

	const epsg2056 = ol.proj.get('EPSG:2056');
	...
</script>
```

We need to create the custom tile grid used by the LV95 vector tiles defined [here](/integration/index.html#vector-tiles-basemap-in-lv95). The number of resolutions must be the exact same as the vector tiles zoom levels to make sure OpenLayers will fetch tiles from all zoom levels but not beyond. If the map instance has finer resolution, we want OpenLayers to over zoom from the last vector tiles zoom level.

```html
<script>
	...
	const vtResolutions = [65536, 32768, 16384, 8192, 4096, 2048, 1024, 512, 256, 128, 64, 32, 16, 8, 4, 2, 1, 0.5];

	const tileGrid = new ol.tilegrid.TileGrid({
		origin: [2420000, 1350000],
		resolutions: vtResolutions,
		sizes: [
			[1, 1],
			[1, 1],
			[1, 1],
			[1, 1],
			[1, 1],
			[1, 1],
			[2, 2],
			[4, 3],
			[8, 6],
			[16, 12],
			[32, 24],
			[63, 48],
			[126, 96],
			[252, 191],
			[503, 381],
			[1006, 762],
			[2012, 1524]
		],
		tileSize: 512,
	});
	...
</script>
```

Then, we initiate a map instance with a view in the LV95 coordinate system and with resolutions going down to `0.015625` to be able to zoom down to scale 1:59. As described above, OpenLayer will over zoom the vector tiles for the zoom levels between the tile grid resolution and the map resolution.

```html
<script>
	...
	const viewResolutions = [65536, 32768, 16384, 8192, 4096, 2048, 1024, 512, 256, 128, 64, 32, 16, 8, 4, 2, 1, 0.5, 0.25, 0.125, 0.0625, 0.03125, 0.015625]

	const map = new ol.Map({
		target: 'map',
		pixelRatio: 2,
		view: new ol.View({
			projection: epsg2056,
			center: [2660000, 1190000],
			zoom: 7,
			resolutions: viewResolutions
		}),
	});
	...
</script>
```

Finally, we need to create a new vector tile layer in OpenLayers with the LV95 coordinate system and the tile grid described above, apply a `style.json` to it using the ol-mapbox-style function `olms.applyStyle` and add it to the map instance. This way, it's possible to create a vector tile layer in a custom coordinate system and a custom tile grid.

```html
<script>
	...
	const layer = new ol.layer.VectorTile({
		declutter: true,
		source: new ol.source.VectorTile({
			projection: epsg2056,
			format: new ol.format.MVT(),
			tileGrid: tileGrid,
			tileSize: 512,
			url: "https://vt.asit-asso.ch/tiles/asit.cadastral.hires/v0.1/2056.asit/{z}/{x}/{y}.mvt"
		}),
	});

	olms.applyStyle(
		layer, 
		"https://vt.asit-asso.ch/styles/2056.asit/asit.cadastryle/style.json", 
		"asit.cadastral", 
		{ resolutions: vtResolutions }
	);

	map.addLayer(layer);
</script>
```