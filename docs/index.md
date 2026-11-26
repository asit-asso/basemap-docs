# ASIT Vector Tiles Basemap

The ASIT Vector Tiles Basemap Service delivers map tiles and styles through a RESTful API. The vector tiles are provided according to the [Mapbox Vector Tile Specification](https://mapbox.github.io/vector-tile-spec/) while the map styles implement the [MapLibre Style Specification](https://maplibre.org/maplibre-style-spec/). The service delivers vector tiles in `EPSG:3857` (Web Mercator) and `EPSG:2056` (LV95) for Switzerland.

!!! Tip "Quick Start"

	Most of the tools can implement vector tiles basemap by simply reading a ``style.json`` file. Use the following endpoint to get a certain style, replacing ``{crs}`` by the desired CRS EPSG code (3857 or 2056) and the `{style}` by the desired style (see below):

	```python
	https://vt.asit-asso.ch/styles/{crs}/{style}/style.json
 
	```

## Styles

5 different styles are provided in both coordinate reference systems and available under the following endpoint:

```
https://vt.asit-asso.ch/styles/{crs}/{style}/style.json
```
With the following parameters :

| Parameter | Values and description |
|---|---|
| ``style`` | `asit.color` : Color topographic basemap <small>Contains an hillshade WMTS layer from swisstopo </small> <br> `asit.light` : Light (greyscale like) topographic basemap <small>Contains an hillshade WMTS layer from swisstopo </small> <br> `asit.dark` : Dark (dark mode) topographic basemap <br> `asit.hybrid` : Hybrid topographic basemap with imagery as background <small>Contains an orthophoto WMTS layer from swisstopo </small><br> `asit.cadastral` : Cadastral basemap based on surveying data (covers only cantons of GE, VD, VS, FR, BE)|
| ``crs`` | `3857` : Web Mercator coordinate system <br> `2056` : LV95 coordinate system. <small>The LV95 styles rely on LV95 tilesets using a custom tile grid [see integration](/integration)</small> |

Example request to get the color basemap in EPSG:2056 (LV95):

```
https://vt.asit-asso.ch/styles/2056/asit.color/style.json
```

!!! Info

	All information to fetch the vector tiles is embedded into the `style.json` file. It points to the `tiles.json` file, himself pointing to the z/x/y tiles endpoint. This is why for common tools, reading the ``style.json`` file is enough to implement a basemap.

## Tileset

2 different tilesets are provided in both coordinates reference systems. Each set has a metadata json file, the `tiles.json` file available under the following endpoint :

```
https://vt.asit-asso.ch/tiles/{tileset}/{version}/{crs}/tiles.json
```
With the following parameters :

| Parameter | Values and description |
|---|---|
| ``tileset`` | `asit` : Tiles encoding topographic features used by ``color``, ``light``, ``dark`` and ``hybrid`` basemap <br> `asit.cadsatral` : Tiles encoding cadastral features used by ``cadastral`` basemap. Covers only cantons GE, VD, VS, FR, BE.|
| ``version`` | `v1.0` : Version of tileset |
| ``crs`` | `3857` : Web Mercator coordinate system <br> `2056` : LV95 coordinate system. <small>The LV95 tilesets use a custom tile grid [see integration](/integration)</small> |

!!! Info

	The `tiles.json` contains metadata about a tileset, i.e. tileset name, description, attribution, information about the features layers (fields and values) and the z/x/y endpoint the get the tiles. For the LV95 tileset, the custom tile grid is also describe in that file.

## Tiles

Tiles from the 2 above tilesets are available under the following endpoint :

```
https://vt.asit-asso.ch/tiles/{tileset}/{version}/{crs}/{z}/{x}/{y}.mvt
```
With the following parameters :

| Parameter | Values and description |
|---|---|
| ``tileset`` | `asit` : Tiles encoding topographic features used by ``color``, ``light``, ``dark`` and ``hybrid`` basemap <br> `asit.cadsatral` : Tiles encoding cadastral features used by ``cadastral`` basemap. Covers only cantons GE, VD, VS, FR, BE.|
| ``version`` | `v1.0` : Version of tileset |
| ``crs`` | `3857` : Web Mercator coordinate system <br> `2056` : LV95 coordinate system. <small>The LV95 tilesets use a custom tile grid [see integration](/integration)</small> |
| ``z`` | `0-16` : zoom levels in the tile grid for the tileset `asit` <br> `0-17` : zoom levels in the tile grid for the tileset `asit.cadastral` |
| ``x`` | Tile column (x coordinate in the tile grid) |
| ``y`` | Tile column (y coordinate in the tile grid) |

## Fonts & Glyphs

All fonts used in basemaps are available as `.pbf` files (glyphs) to be read by modern vector tiles client and as standard web font (`.woff2` or `.ttf`) for interoperability with other client. They are shared across styles and have their own endpoint.

Glyphs are available under the following endpoint :

```
https://vt.asit-asso.ch/fonts/{fontstack}/{range}.pbf
```

Where the `fontstack` contains the font family, fontweight and fontstyle and the `range` is used to select the right `.pbf` file.

Standard web fonts are reference in `.css` files for interoperability and are available under the following endpoint :

```
https://vt.asit-asso.ch/fonts/web/{Font+Family}/{fontweight}{-fontstyle}.css
```

Where the font family, fontweight and fontstyle are 3 separated parameters.

## Sprite

Some of the basemaps use icons. Icons are encoded as sprite and are available at the level of a `style.json` under the following endpoints:

Every sprite consists of `.png` file containing all icons and a `.json` file referencing their position.
```
https://vt.asit-asso.ch/styles/{crs}/{style}/sprite.png
https://vt.asit-asso.ch/styles/{crs}/{style}/sprite.json

```

Icons for high resolution screen
```
https://vt.asit-asso.ch/styles/{crs}/{style}/sprite@2x.png
https://vt.asit-asso.ch/styles/{crs}/{style}/sprite@2x.json
```