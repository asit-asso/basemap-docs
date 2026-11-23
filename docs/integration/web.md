# Web

## MapLibre GL JS
MapLibre is one of the most performant client to render vector tiles basemap but does not work with the swiss LV95 coordinate system.

After integrating the necessary dependencies and creating a `map` container, you simply need to point to a `style.json` file :


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

First integrate the necessary OpenLayers and ol-mapbox-style dependencies :

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

ToDo