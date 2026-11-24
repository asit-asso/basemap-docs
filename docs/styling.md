# Customizing the style
One big advantage of vector tiles is that the rendering is made on client side. It's therefore very light to implement new styles to create different basemaps. All you need is to set up a new styles that consumes the same vector tiles data stream. 

You can then : 

* Remove objects based on attributes or even entire layer (see [data schema](./schema.md))
* Change objects color, width, opacity and other paintings properties based (or not) on zoom level
* Apply offset to geometries
* Change the way the labelling is done (placement, size, text, ...)
* Change the way the icons are rendered (placement, size, ...)
* Add your own raster or vector data source and mix it with our tileset
* and much more, actually everything that the [MapLibre Style Specification](https://maplibre.org/maplibre-style-spec/) and our [data schema](./schema.md) support

To have a glimpse of what's possible, copy your favorite `style.json` (see [styles](./index.md#styles)) file locally and load it into the [maputnik](https://maputnik.github.io/) application. Only works with Web Mercator tileset !

Once you are happy with your own `style.json` file, either keep it locally for your desktop app or upload it on your own server for your web application. You can also tweak the style at runtime with the [MapLibre GL JS](https://maplibre.org/maplibre-gl-js/docs/) or [OpenLayers](https://openlayers.org/) libraries.

**No inspiration ?** Try our [style generator](./demo.md#basemaps-in-epsg3857) !

