# Third-party data and services

GeoLandscape contains no third-party code beyond the Unreal Engine modules it links
(PROJ, LibTiff, zlib and others shipped with the engine). It downloads open geographic
data while you generate a region. The generated landscapes, roads, buildings and forests
are derived from that data, so **your project must credit the sources it used**, usually
in the game credits. The plugin records the elevation source of every tile on the region
grid actor (`GeoTileWorld` → Tiles → Elevation Source / Attribution).

The terms below are a summary. Check the linked pages for the current terms before you
ship.

## Map data

| Source | Used for | Terms | Credit line |
|---|---|---|---|
| OpenStreetMap | Roads, buildings, rivers, lakes, forests, place search | [ODbL 1.0](https://www.openstreetmap.org/copyright) | © OpenStreetMap contributors |
| OpenFreeMap / OpenMapTiles vector tiles | Fallback OpenStreetMap source when Overpass is unavailable; woodland shown on the distant horizon | [OpenFreeMap](https://openfreemap.org/), [OpenMapTiles](https://openmaptiles.org/) | OpenFreeMap © OpenMapTiles Data from OpenStreetMap |

The OpenStreetMap data in a game is a *produced work* under the ODbL: credit is
required, the game itself does not have to be open. If you redistribute the extracted
vector data as a database, the ODbL share-alike terms apply to that database.

## Elevation data

| Source | Region | Terms | Credit line |
|---|---|---|---|
| Copernicus DEM GLO-30 | Worldwide | [Copernicus DEM licence](https://spacedata.copernicus.eu/collections/copernicus-digital-elevation-model) | Produced using Copernicus WorldDEM-30 © DLR e.V. 2010-2014 and © Airbus Defence and Space GmbH 2014-2018 provided under COPERNICUS by the European Union and ESA |
| USGS 3D Elevation Program | United States | Public domain | U.S. Geological Survey, 3D Elevation Program |
| swissALTI3D | Switzerland | [Swiss open government data](https://www.swisstopo.admin.ch/en/terms-of-use-free-geodata-and-geoservices) | © swisstopo |
| Terrarium elevation tiles (Mapzen / Tilezen) | Fallback, worldwide; distant horizon terrain around every region | [Attribution list](https://github.com/tilezen/joerd/blob/master/docs/attribution.md) | As listed for the covered area |
| Mapbox Terrain-RGB | Optional, needs your token | Your Mapbox agreement | Check that your plan allows storing the data in a game |
| Your own GeoTIFF | Anywhere | Your data's licence | As required by your data |

## Ground textures

The ground material (`Content/Textures/Ground`) uses textures from
[Poly Haven](https://polyhaven.com/license), released under
[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/): no credit is required.
The colour and DirectX normal maps are used at 2K, unmodified.

| Plugin texture | Poly Haven asset | Author |
|---|---|---|
| T_GeoGround_Grass | [Forrest Ground 01](https://polyhaven.com/a/forrest_ground_01) | Rob Tuytel |
| T_GeoGround_DryGrass | [Leafy Grass](https://polyhaven.com/a/leafy_grass) | Charlotte Baglioni |
| T_GeoGround_Meadow | [Aerial Grass Rock](https://polyhaven.com/a/aerial_grass_rock) | Rob Tuytel |
| T_GeoGround_Soil | [Forest Ground 04](https://polyhaven.com/a/forest_ground_04) | Rob Tuytel, Rico Cilliers |
| T_GeoGround_Rock | [Lichen Rock](https://polyhaven.com/a/lichen_rock) | Rico Cilliers |
| T_GeoGround_Cliff | [Aerial Rocks 02](https://polyhaven.com/a/aerial_rocks_02) | Rob Tuytel |
| T_GeoGround_Sand | [Coast Sand 01](https://polyhaven.com/a/coast_sand_01) | Rob Tuytel |
| T_GeoGround_Snow | [Snow 02](https://polyhaven.com/a/snow_02) | Rob Tuytel |

The stylized set (`Content/Textures/GroundStylized`) is original work made for the plugin
with its own procedural painting script; it contains no third-party content.

## Online services

The editor contacts these services while you work. Each has a usage policy that the
plugin follows (identifying User-Agent, cached responses, limited request rates):

- Overpass API instances (configurable in Project Settings > Plugins > GeoLandscape)
- [Nominatim](https://operations.osmfoundation.org/policies/nominatim/) for place search
- [OpenStreetMap tile server](https://operations.osmfoundation.org/policies/tiles/) for the region picker map
- OpenFreeMap vector tiles, AWS Open Data (Copernicus DEM, Terrarium), USGS and swisstopo
  elevation services

None of these services is contacted by a packaged game: the runtime module only contains
the generated actors.
