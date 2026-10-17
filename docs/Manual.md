# GeoLandscape for Unreal Engine 5.8

GeoLandscape turns any place on Earth into a playable World Partition level inside the
Unreal Editor. Pick an area on the map, press **Create region**, and the plugin builds:

- a Landscape from open elevation data at real 1:1 scale (100 Unreal units per metre),
- OpenStreetMap roads that follow the terrain, with the ground levelled under them,
- buildings from real footprints and heights,
- riverbeds carved into the terrain, and optional UE Water oceans, lakes and rivers,
- PCG forests placed from mapped woodland, with editable vegetation palettes,
- a far-view HLOD so cities, roads and forests stay visible to the horizon,
- distant terrain about 30 km around the region, so its edges never end in empty space.

Large regions are generated as streamed 2 × 2 km tiles, one pass at a time, and can be
resumed or extended later without rebuilding what is already finished.

Русская версия: [Manual.ru.md](Manual.ru.md).

## Contents

1. [Requirements](#requirements)
2. [Installation](#installation)
3. [Quick start](#quick-start)
4. [The GeoLandscape panel](#the-geolandscape-panel)
5. [Elevation sources](#elevation-sources)
6. [Tiled generation, resume and extension](#tiled-generation-resume-and-extension)
7. [OpenStreetMap data and network](#openstreetmap-data-and-network)
8. [Ground material](#ground-material)
9. [Forests and vegetation palettes](#forests-and-vegetation-palettes)
10. [Distant view (HLOD) for the game](#distant-view-hlod-for-the-game)
11. [Roads and buildings](#roads-and-buildings)
12. [Project settings](#project-settings)
13. [Console commands](#console-commands)
14. [Demo levels](#demo-levels)
15. [Performance and limits](#performance-and-limits)
16. [Troubleshooting](#troubleshooting)
17. [Known issues](#known-issues)
18. [Help and support](#help-and-support)
19. [Data attribution](#data-attribution)

## Requirements

- Unreal Engine 5.8, Windows 64-bit editor.
- A C++ project, or a Blueprint project to which you add one C++ class so the plugin can be
  compiled (Fab installs the plugin precompiled for the engine version you download it for).
- Engine plugins enabled automatically by GeoLandscape: PCG, Water, Procedural Mesh
  Component, GeoReferencing.
- An internet connection while generating. Downloaded data is cached, so resuming or
  regenerating the same area works offline.
- Recommended: 32 GB RAM and an SSD for regions larger than about 10 × 10 km.

Nothing else is required: GeoTIFF, coordinate transforms and compression use the
libraries that ship with the engine (libtiff, PROJ, SQLite, zlib).

## Installation

1. Install the plugin from Fab into your engine, or copy the `GeoLandscape` folder into
   your project's `Plugins` folder.
2. Open the project and enable **GeoLandscape** in **Edit → Plugins** if it is not enabled.
3. Restart the editor.
4. Open the panel with the **GeoLandscape** button on the main toolbar, next to **Play**,
   or press **Ctrl+Alt+G**. It is also in **Window** and **Tools**, and the
   `GeoLandscape.Open` console command opens it too.

The arrow next to the toolbar button opens a menu with the examples, the region actions
(resume, distant view, horizon), the ground style and this manual.

The plugin's own content (default trees, shrubs, grass and rocks, the default vegetation
palette and the demo levels) is visible in the Content Browser under
**Plugins → GeoLandscape Content** once **Show Plugin Content** is enabled in the view
options.

## Quick start

1. Press **GeoLandscape** on the toolbar (**Ctrl+Alt+G**).
2. Search for a place (for example `Zermatt`) or click the map. Drag to pan, use the
   wheel to zoom, Shift+drag to draw an area. The example pictures under **Place and size**
   select Mount Everest, the Matterhorn, San Francisco or Geirangerfjord in one click.
3. Set **Width** and **Length** in kilometres. Start with 4 × 4 km; the badge next to them
   shows the number of 2 × 2 km tiles.
4. Under **What to create**, keep **Roads**, **Buildings** and **Forest and grass** on.
5. Press **Create region**.

GeoLandscape saves a new World Partition map in `/Game/GeoLandscape/Regions/` and builds
the region in three passes: terrain with riverbeds, then the city and roads, then the PCG
forest. Each finished pass of each tile is saved and unloaded, so the editor's memory stays
flat even for large regions. When it finishes:

- press **Show finished tiles** in the **Region on this map** card to load the whole
  region for viewing, and
- press **Distant view** before playing, so distant tiles show their HLOD instead of
  empty ground.

## The GeoLandscape panel

The map is on the left. On the right, two steps (**Place and size**, **What to create**)
lead to **Create region**. The **Region on this map** card appears when the open map holds
a region: it shows the progress of each pass and has **Stop**, **Resume**, **Show finished
tiles**, **Distant view** and **Horizon**. Hover over any option for an explanation.

**Map and search.** The search accepts place names (resolved through OpenStreetMap
Nominatim) and coordinates such as `45.9763, 7.6586`. With **Only inside the outline of
the found place**, roads and buildings are limited to the found city or district.

**Region size.** Width and length are measured on the ground. Tiled mode (the default)
splits the area into 2 × 2 km World Partition tiles with a shared grid of about 2 m per
vertex. Single Landscape mode (**More → 2 × 2 km tiles** off, or in **Advanced settings**)
replaces the GeoLandscape terrain of the current map with one Landscape, which suits small
areas only.

**What to create.**

| Option | Result |
|---|---|
| Riverbeds | Riverbeds and lake beds carved into the terrain. The terrain itself is always created. |
| Roads | Road meshes conformed to the terrain; the ground is levelled across each road. Bridges and tunnels keep their own height. |
| Buildings | Extruded OSM footprints with real or estimated heights, batched per sector. |
| Forest and grass | PCG forests inside mapped woodland, with understory, using the vegetation palette; Landscape grass on meadows. |
| UE water (sea, lakes) | Water Body actors of the Water plugin with one shared water zone. |
| Horizon 30 km | Low-detail terrain for about 30 km around the region. |

**Ground style** picks the realistic or the hand-painted texture set (see below).

**More** next to **Create region**: resume an unfinished region, switch between tiles and
one Landscape, clear the download cache.

**Advanced settings** (collapsed by default, also opened by **Settings** in the panel
header): elevation source, terrain spacing, river depth, forest density, tree model, the
vegetation palette tools, the city reference visibility and the download cache.

## Elevation sources

| Source | Where | Resolution | Key |
|---|---|---|---|
| **Auto** (default) | Picks the best open source below | best available | none |
| USGS 3DEP | United States | 1–10 m bare earth | none |
| swissALTI3D | Switzerland | 2 m bare earth | none |
| Copernicus GLO-30 | Worldwide | 30 m surface model | none |
| AWS Terrarium | Worldwide, fallback | varies by region | none |
| Mapbox Terrain-RGB | Worldwide | varies | your token |
| GeoTIFF | Your file | your data | — |

Auto tries the regional source first and falls back to Copernicus and then Terrarium.
Copernicus is a surface model (trees and buildings can raise it); USGS and swissALTI3D are
bare-earth models. A denser Landscape grid smooths but does not add detail that the source
does not have.

**GeoTIFF** accepts single-band numeric heights (Float32/64, Int16/32) with
georeferencing and an EPSG code, in TIFF or BigTIFF, local or as a cloud-optimised URL.
Colour maps and areas outside the file are rejected.

**Mapbox:** Mapbox's terms may not allow storing their elevation in a game level. Check
your plan before shipping a level built from it.

A region keeps the elevation source it was created with, so extending it never mixes
height datums along tile edges. Start a new region to use a different source.

## Tiled generation, resume and extension

- **Three passes.** All tiles get terrain and riverbeds first, then the city and roads,
  then forests. Each kind of OpenStreetMap data is downloaded once for the whole area and
  shared by its tiles.
- **Resume.** If the network fails or you press **Stop and keep progress**, every finished
  pass stays on disk. Open the region map and press **Resume unfinished region**:
  finished passes are skipped.
- **Extend.** Open a saved region, select a neighbouring area and press **Create region**.
  New tiles snap to the existing grid; finished tiles are skipped and the shared edges
  match.
- **Where data lives.** The region map and its `__ExternalActors__` /
  `__ExternalObjects__` folders form the level. Downloaded and processed source data is
  cached in `Saved/GeoLandscape/` so a region can be resumed without downloading again.
  Keep both when you move a project.

**Horizon.** After the last pass, GeoLandscape downloads coarse elevation for about 30 km
around the region and builds one always-loaded mesh, `<Map>_Horizon`, that meets the
region's edge heights exactly and keeps the slope of the terrain across the edge. Open sea
around coastal regions becomes a flat water surface, and woodland mapped in OpenStreetMap
(OpenFreeMap vector tiles) is shown as forest canopy colour; no trees are placed there. A
region that has only its terrain pass shows no forest yet, so the canopy then stops at its
edge until the region's forests are generated. `GeoLandscape.BuildHorizon` rebuilds the
horizon, for example for regions made with older versions.

**Sea.** Surface models such as Copernicus store the open sea as a flat plane at 0 m, so a
UE Water ocean would lie on the ground and stay invisible. Wide areas that are exactly flat
at sea level get a sea bed from 1.5 m deep at the shore to 12 m offshore; land below sea
level, such as polders, has relief and is left as it is.

**Water in a tiled region.** The region has one UE Water zone and one ocean, both always
loaded and grown with the region; rivers and lakes stream with their tile. Rivers keep
their OpenStreetMap width; untagged streams are 2.5 m wide and ditches 1.5 m, and every
channel is carved at least one landscape cell deep, so a stream lies below its banks. The
water flows faster where the river falls, and where it falls by more than about 30 % the
water body is replaced by a white-water cascade (`M_GeoCascade`), so waterfalls on steep
slopes look like falling water instead of a tilted turquoise ribbon. The ocean continues past
the region to beyond the horizon (the water zone's far mesh), so the sea does not end at the
edge of the generated area; for a region made with an older version, press **Horizon** in
the panel or run `GeoLandscape.BuildHorizon`.

World Z = 0 is sea level. Before the first tile, the plugin measures the elevation of the
whole area and picks the height range of the region: normally 12.5 cm steps from −2048 m
to 6143 m, shifted for high mountains such as Everest or Denali, and 25 cm steps only when
a single region spans more than about 8 km of height. Extending a region later keeps its
range.

## OpenStreetMap data and network

Roads, buildings, water and forests come from OpenStreetMap through the Overpass API.
The plugin follows the public services' usage policies: it identifies itself with a
User-Agent, caches every response for 30 days and limits its request rate.

**Overpass servers.** Public Overpass servers are sometimes overloaded or blocked by a
network. The list of servers is in **Project Settings → Plugins → GeoLandscape →
Overpass API servers**; when one does not respond, the next one is used. You can add any
other Overpass instance, including your own. Before a region's first download the plugin
asks all servers a trivial query at once; if none answers within about 12 seconds, the
vector tiles below take over straight away instead of after minutes of retries.

**Vector tile fallback.** When no Overpass server responds, GeoLandscape reads the same
OpenStreetMap data from vector tiles (OpenFreeMap, OpenMapTiles schema) and generation
continues. The tile data is slightly simpler: roads have no width or lane tags and
buildings no level count, so the plugin's defaults are used for those. You can disable the
fallback or point it to another OpenMapTiles source in the project settings.

**Terrain without OpenStreetMap.** If river outlines cannot be loaded at all, the terrain
is still created; riverbeds are carved later in the roads pass once the data is
available.

## Ground material

Every region gets an instance of the generated ground material `M_GeoLandscape`
(`/Game/GeoLandscape/M_GeoLandscape`, instances in `Generated/`). It needs no painting:
slope, height, the forest layer and the coastline decide where grass, dry grass, soil,
forest floor, rock, sand, snow and sea floor appear. The snow line and tree line are set
from the region's latitude (about 3000 m and 2150 m in the Alps, 1300 m and 750 m in
southern Norway).

The layers use CC0 textures that ship with the plugin (`Textures/Ground`). Tiling is hidden
by a second, rotated texture scale that takes over with distance and by aerial textures
far away; cliffs use triplanar projection.

To restyle the ground, open the region's `MI_GeoLandscape_*` instance (or the parent to
change every region):

- **04 Layer Colors** sets the average colour of each layer. Textures only add detail
  around it, so you can swap a texture without retuning colours.
- **10 Color Textures / 11 Normal Textures** take your own textures, for example
  Megascans. Any tileable colour and DirectX normal map works.
- **05 Layer Sizes** sets the real-world size of one texture repeat.
- **02 Coverage** controls rock slope, bare soil, dry grass and large-scale variation;
  **01 Terrain** holds the sea level, snow line and tree line.

**Stylized ground.** **Ground style → Hand-painted** in the panel switches to a
hand-painted texture set (`Textures/GroundStylized`) with a brighter palette, softer detail
and larger brush-stroke repeats, which suits the low-poly vegetation. It applies to new
regions and switches the region of the open map at once, including its distant view and
horizon; `GeoLandscape.GroundStyle stylized | realistic` does the same from the console.
The style is stored as overrides on the region's material instance.

After a plugin update, `GeoLandscape.UpdateGroundMaterial` brings the regions of the open
map to the new ground material, including their distant view, without regenerating
anything.

## Forests and vegetation palettes

Forests are PCG components placed inside mapped woodland, scrub, parks and grassland.
The plugin ships a default palette, `DA_GeoDefaultForest`, with low-poly pine, broadleaf
and birch trees, shrubs, grass and rocks that use one vertex-colour material. Replace them
with your own models:

- **One forest:** select a `GeoVegetation` actor in the Outliner, press **Configure selected
  forest**, change the species, shares, heights and density, then press **Apply changes
  to this forest**. Neighbouring forests keep their own vegetation.
- **Many forests:** create or edit a **vegetation palette** and apply it to the selected
  forests or to every forest on the map.

Trees grow only inside mapped woodland. A mapped forest gets about 160 trees per hectare,
thinned into groves and small glades, and an area that OpenStreetMap maps twice (for
example `landuse=forest` over `natural=wood`) is planted once. The palette's understory
(shrubs, grass and stones) covers the glades and part of the floor under the trees.

**Grass.** The ground material has a Landscape Grass output with `LGT_GeoGrass`: the
engine scatters grass tufts on open grass, fewer on dry patches, slopes and forest floor,
within about 60 m of the camera. Nothing is stored in the map. Change the density, the
distance or the mesh in `LGT_GeoGrass`, or turn grass off with `grass.Enable 0`.

Lowering the density does not delete the original points, so it can be raised again.
Each forest is backed up to `Saved/GeoLandscape/ForestBackups/` before it is replaced.
After changing models, rebuild the distant view.

## Distant view (HLOD) for the game

Press **Build the distant view for the game** after generating or changing tiles. It
builds World Partition HLODs inside the running editor:

- Terrain uses a far material without baking textures.
- Road and building sectors are merged into simplified meshes (parallel quadric
  reduction).
- Forests use instanced HLODs with a solid canopy material for masked foliage.

The build runs in batches sized from the free memory, with a save and a garbage
collection between them, so even regions of hundreds of square kilometres build without
exhausting memory. Unchanged cells are skipped through their source hash. A region of
about 12 × 8 km builds in a few minutes.

`GeoLandscape.BuildDistantView box=MinX,MinY,MaxX,MaxY` rebuilds only the cells inside a
box (world units).

## Roads and buildings

Generated roads and buildings are grouped into **sector actors** per tile and area. Each
sector offers editor actions in the Details panel:

- **Expand into actors** turns a sector into individually editable road or building actors;
  **Collapse back** batches them again.
- **Material overrides** replace the road or building material for a whole sector.
- Buildings can be rebuilt with another mesh or height from the building editor.

Road and building geometry is stored in a compact binary format: float positions and
8-bit normals (exactly what rendering and collision use), compressed with Oodle. Compared
with plain procedural mesh properties, sectors are about 10 times smaller and load
correspondingly faster; the San Francisco demo's city went from 632 MB to 68 MB. Regions
created with older versions are converted with `GeoLandscape.CompactCityStorage`.

## Project settings

**Project Settings → Plugins → GeoLandscape** (stored per user, since server reachability
depends on your network):

| Setting | Default | Purpose |
|---|---|---|
| Overpass API servers | overpass-api.de, overpass.private.coffee, maps.mail.ru | Servers tried in order for OSM data |
| Use vector tiles when Overpass is unavailable | on | Automatic fallback described above |
| Vector tile source | `https://tiles.openfreemap.org/planet` | TileJSON or `{z}/{x}/{y}` URL in the OpenMapTiles schema |

## Console commands

| Command | Purpose |
|---|---|
| `GeoLandscape.Open` | Open the panel. |
| `GeoLandscape.GenerateRegion <lat> <lon> <width-km> <height-km> [water]` | Generate exactly as the Create button does, for scripted builds. `water` adds UE Water bodies. |
| `GeoLandscape.ResumePlannedTiles` | Resume the saved plan of the open region. |
| `GeoLandscape.BuildDistantView [noprompt] [box=...]` | Build the HLOD distant view. |
| `GeoLandscape.CompactCityStorage` | Convert all road and building sectors of the open map to the compact storage. |
| `GeoLandscape.BuildHorizon` | Build or rebuild the distant terrain around the region of the open map. |
| `GeoLandscape.UpdateGroundMaterial` | Rebuild the ground material if it is outdated and update the open map's regions and their distant view. |
| `GeoLandscape.GroundStyle stylized \| realistic` | Switch the ground of the open map's regions between the hand-painted and the realistic look. |
| `GeoLandscape.BuildDefaultContent` | Rebuild the plugin's default vegetation assets. |
| `GeoLandscape.EnsurePlayerStart X Y Z Yaw` | Add an always-loaded Player Start if the map has none. |

Console variables:

| Variable | Purpose |
|---|---|
| `GeoLandscape.GenerationState` | Read-only: 0 idle, 1 generating, 2 finished, 3 failed. |
| `GeoLandscape.OutputRoot` | Content folder for generated regions (default `/Game/GeoLandscape`). |
| `GeoLandscape.RegionName` | Map name for the next new region. |
| `GeoLandscape.DistantViewBatchMB` | Fixed batch size for the distant view build (0 = automatic). |
| `GeoLandscape.DistantViewRebuildToken` | Change it to force a rebuild of every city and road HLOD. |

Scripted example (editor Python):

```python
import unreal
world = unreal.get_editor_subsystem(unreal.UnrealEditorSubsystem).get_editor_world()
unreal.SystemLibrary.execute_console_command(world, 'GeoLandscape.GenerateRegion 37.795 -122.406 4 4 water')
```

## Demo levels

The plugin includes ready-made regions in **GeoLandscape Content → Demo → Regions**:

| Level | Place | Shows |
|---|---|---|
| `Demo_Everest` | Mount Everest and Lhotse, Nepal / China | 8848 m summit, glaciers, snow line from latitude, Copernicus terrain |
| `Demo_Matterhorn` | Zermatt and the Matterhorn, Switzerland | 2 m swissALTI3D terrain, village, river, forests |
| `Demo_SanFrancisco` | Downtown San Francisco, USA | USGS 3DEP terrain, dense city, bay water |
| `Demo_Geirangerfjord` | Geirangerfjord, Norway | steep coast, fjord water, rivers and forest |

Open a demo, press **Show finished tiles** in the panel or load cells from the World
Partition window, and play to see the streamed tiles and the distant view.

## Performance and limits

- One 2 × 2 km tile has a 1009 × 1009 vertex Landscape. Regions of up to 4096 tiles are
  supported; the practical size depends on the density of the city and forests.
- The editor keeps only the tiles you load; generation and the distant view build work
  tile by tile or batch by batch.
- With the OpenStreetMap data already downloaded, one tile takes about 6 s for terrain and
  about 3 s each for roads and forest: a region of about 1000 km² (250 tiles) takes under
  an hour.
- Real scale means real distances: plan streaming ranges and the distant view for your
  game.
- OSM completeness varies by place. Missing widths and heights are estimated from the road
  and building types.
- The projection is a local Web Mercator approximation scaled at the region's latitude;
  very large regions keep some projection distortion.

## Troubleshooting

**Generation stops with "OpenStreetMap servers (Overpass) are not responding".** The
public servers are overloaded or blocked by your network. Wait and press **Resume
unfinished region**, add another server in the project settings, or keep the vector tile
fallback enabled.

**The editor runs out of memory when you press Play on a large region.** Play in Editor
copies every loaded actor. Unload the cells in the World Partition window, build the
distant view, then play; World Partition streams the tiles around the player. Windows also
needs a page file large enough for the editor's peak use.

**Distant tiles are empty in play.** Build the distant view after generating.

**"Save the map first".** Generation and the distant view work from saved packages; save
the map and run again.

**A forest looks different after editing a palette.** Palettes apply on demand: press
**Apply changes to this forest** or apply the palette to the map, then rebuild the
distant view.

## Known issues

- **UE Water near the camera in play.** In Play In Editor and in Movie Render Queue, the
  UE Water surface may be missing within about 1 km of the camera; farther away it is
  drawn. The editor viewport shows the water correctly. UE Water is optional and off by
  default; the riverbeds and sea floor carved into the terrain are not affected.
- **Trees above the tree line.** OpenStreetMap vegetation areas on high mountains can
  place trees and bushes on snow. Remove them with the forest tools or edit the palette.

## Help and support

- Documentation, screenshots and updates: <https://github.com/evG10/geolandscape-showcase>
- Questions and bug reports: <https://github.com/evG10/geolandscape-showcase/issues>.
  Please include the engine version, the place and size of the region, and the Output Log
  lines that start with `LogGeoLandscape`.

## Data attribution

Generated levels contain data derived from OpenStreetMap and from the elevation source you
used, and must credit them. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for the
credit lines and terms. The source of each tile is recorded on the region's
`GeoTileWorld` actor.
