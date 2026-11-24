Information about data schema used in a tileset is already available in the metadata `tiles.json` file (see [tileset](../index.md#tileset)). Encoded objects are separated into different layers available under the key `vector_layers`. For each of them, fields name and their description and possible values are available.

We give here more detailed information about tilesets data schema.

!!! Info

	Even if a layer is available for a given zoom level, it still may contain no features because we filter out objects that are too small for a given zoom level.

## Tileset `asit`

| Layer | Geometry | Description | Source | min zoom | max zoom | Attributes |
|---|---|---|---|---|---|---|
| `landcover` | Polygon | Land cover (natural surfaces) | swisstopo ``swissTLM3D`` product | 0 | 16 | **`class`**: blockfield, cliff, glacier, open_wood, scree, scrub, snowfield, wetland, wood, wooded_area |
| `landuse` | Polygon | Land use | swisstopo ``swissTLM3D`` and ``swissTMLRegio`` products | 0 | 16 | **`class`**: campsite, caravan_site, cemetery, education, garden, golf, healthcare, horse_racing, leisure_park, orchard, park, pitch, pitch_area, plant_nursery, quarry, residential, swimming_pool_area, vineyard, zoo |
| `waterway` | Line | Rivers, penstocks and lake axes | swisstopo ``swissTLM3D`` product | 5 | 16 | **`class`**: river, penstock, lake_axis<br>**`bridge`**: yes, no<br>**`name`**: river name<br>**`rank`**: ranking based on feature length (lower = more important) |
| `water` | Polygon | Water bodies (lakes & rivers) | swisstopo ``swissTLM3D`` product | 5 | 16 | no attribute |
| `water_pnt` | Point | Lake labels | swisstopo ``swissTLM3D`` product (centroïds of lakes) | 7 | 16 | **`name`**: lake name<br>**`rank`**: ranking based on lake area (lower = more important) |
| `airport` | Polygon | Airport areas | swisstopo ``swissTLM3D`` product | 11 | 16 | no attribute |
| `street` | Line | Roads, paths and ferries | swisstopo ``swiss Map Vector 10`` product for geometries and some attributes and swisstopo `Official street directory` for names | 5 | 16 | **`class`**: motorway, semi_motorway<br><small>from z7 only</small><br> 4m_street, 6m_street, 8m_street, 10m_street, ferry, motorway_link, rest_stop, service_link, trunk_link<br><small>from z13 only</small><br> 3m_street<br><small>from z14 only</small><br> 1m_path, 1m_path_fragment, 2m_path, 2m_path_fragment<br>**`zorder`**: vertical order<br>**`name (from z14 only)`**: street name (languages separated by `/` if multilingual) |
| `railway` | Line | Railways | swisstopo ``swissTLM3D`` product | 8 | 16 | **`zorder`**: vertical order<br>**`name (from z14 only)`**: railway name |
| `parking` | Polygon | Parking areas | swisstopo ``swissTLM3D`` product | 14 | 16 | no attribute |
| `building` | Polygon | Buildings | official surveying data for cantons GE, VD, VS, FR and BE and swisstopo ``swissTLM3D`` product for the rest | 12 | 16 | **`underground`**: yes, no |
| `railway_platform` | Polygon | Railway platforms | swisstopo ``swissTLM3D`` product | 14 | 16 | no attribute |
| `boundary` | Polygon | Administrative boundaries | swisstopo ``swissBOUNDARIES3D`` product | 0 | 16 | **`class`**: country, canton |
| `address` | Point | Building entrances (house numbers) | official surveying data for cantons GE, VD, VS, FR and BE and swisstopo ``Official directory of building addresses`` product for the rest | 16 | 16 | **`housenumber`**: building entrance number<br>**`orientation`**: Building entrance number rotation. Given in degree (CW from east = 0) |
| `place` | Point | Place names | swisstopo ``swissTLM3D`` product for geometries and attributes and swisstopo `swissBOUNDARIES3D` for some attributes | 4 | 16 | **`class`**: location<br><small>from z14 only</small><br> suburb, quarter, neighbourhood, locality<br>**`name`**: default name<br>**`population`**: >100_000<br><small>from z6 only</small><br> 50_000-100_000<br><small>from z7 only</small><br>10_000-49_999<br><small>from z10 only</small><br>2000-9999<br><small>from z11 only</small><br> 1000-1999<br><small>from z12 only</small><br>100-999<br><small>from z13 only</small><br>50-99<br><small>from z14 only</small><br> 20-49, <20<br>**`rank`**: ranking based on population (lower = more important) |

## Tileset `asit.cadastral`

| Layer | Geometry | Description | Source | min zoom | max zoom | Attributes |
|---|---|---|---|---|---|---|
| `landcover_water` | Polygon | Water land cover | official surveying data | 0 | 17 | no attribute |
| `landcover_wood` | Polygon | Wooded land cover | official surveying data | 0 | 17 | no attribute |
| `landcover_artificial` | Polygon | Artificial land cover | official surveying data | 11 | 17 | **`class`**: street<br><small>from z15 only</small><br> other |
| `misc_object_ply` | Polygon | Miscellaneous objects (areas) | official surveying data | 12 | 17 | **`class`**: bridge<br><small>from z16 only</small><br> water, other |
| `building_underground` | Polygon | Underground buildings | official surveying data | 14 | 17 | no attribute |
| `building` | Polygon | Buildings | official surveying data | 13 | 17 | no attribute |
| `freestanding_roof` | Polygon | Freestanding roofs | official surveying data | 13 | 17 | no attribute |
| `misc_object_lin` | Line | Miscellaneous objects (lines) | official surveying data | 12 | 17 | **`class`**: railway<br><small>from z15 only</small><br> path<br><small>from z16 only</small><br> sidewalk, water, other |
| `parcel` | Polygon | Parcels | official surveying data | 16 | 17 | no attribute |
| `public_land` | Polygon | Public land | official surveying data | 16 | 17 | no attribute |
| `boundary` | Polygon | Administrative boundaries | swisstopo ``swissBOUNDARIES3D`` product | 0 | 17 | **`class`**: country, canton<br><small>from z8 only</small><br> municipality |
| `boundary_point` | Point | Boundary points | official surveying data | 17 | 17 | **`class`**: marker, pin, cross_mark, stake |
| `planimetric_fixed_point_3` | Point | Planimetric fixed points (category 3) | official surveying data | 17 | 17 | **`class`**: marker, pin, cross_mark, stake |
| `planimetric_fixed_point_2` | Point | Planimetric fixed points (category 2) | official surveying data | 17 | 17 | **`class`**: accessible, inaccessible |
| `planimetric_fixed_point_1` | Point | Planimetric fixed points (category 1) | official surveying data | 17 | 17 | **`class`**: accessible, inaccessible |
| `altimetric_fixed_point_1` | Point | Altimetric fixed points (category 1) | official surveying data | 17 | 17 | no attribute |
| `altimetric_fixed_point_2` | Point | Altimetric fixed points (category 2) | official surveying data | 17 | 17 | no attribute |
| `street_label` | Line | Street labels | official surveying data | 14 | 17 | **`name`**: street name |
| `landcover_label` | Point | Land cover labels | official surveying data | 17 | 17 | **`name`**: label name (e.g. cantonal road, school, river, ...)<br>**`orientation`**: label rotation. Given in degree (CW from east = 0) |
| `building_underground_label` | Point | Underground building numbers | official surveying data | 17 | 17 | **`building_number`**: official underground building number (e.g. ECA number for canton Vaud)<br>**`orientation`**: number rotation. Given in degree (CW from east = 0) |
| `building_label` | Point | Building numbers | official surveying data | 17 | 17 | **`building_number`**: official building number (e.g. ECA number for canton Vaud)<br>**`orientation`**: number rotation. Given in degree (CW from east = 0) |
| `freestanding_roof_label` | Point | Freestanding roof numbers | official surveying data | 17 | 17 | **`building_number`**: official freestanding roof number (e.g. ECA number for canton Vaud)<br>**`orientation`**: number rotation. Given in degree (CW from east = 0) |
| `address` | Point | Building entrances (house numbers) | official surveying data | 17 | 17 | **`house_number`**: building entrance number<br>**`orientation`**: number rotation. Given in degree (CW from east = 0) |
| `parcel_label` | Point | Parcel numbers | official surveying data | 17 | 17 | **`parcel_number`**: parcel number<br>**`orientation`**: number rotation. Given in degree (CW from east = 0) |
| `public_land_label` | Point | Public land numbers | official surveying data | 17 | 17 | **`land_number`**: public land number<br>**`orientation`**: number rotation. Given in degree (CW from east = 0) |
| `municipality_label` | Point | Municipality names | swisstopo ``swissBOUNDARIES3D`` product | 5 | 16 | **`name`**: municipality name<br>**`rank`**: ranking based on population (lower = more important) |
