# GeoLandscape Case Study

## Goal

GeoLandscape was built as an Unreal Engine 5 editor plugin for quickly turning real geodata into a playable scene: terrain, city structure, roads, buildings, rivers and water areas.

The practical goal was not just to import a heightmap. The plugin had to create an editable Unreal scene where generated roads and buildings can be inspected, changed and regenerated without losing all manual work.

## Main Problem

A raw terrain heightmap is not enough for a useful environment. A city scene also needs:

- correct real-world scale;
- city boundaries;
- roads that sit on the terrain instead of floating or clipping;
- buildings from footprints and approximate heights;
- rivers and water areas;
- materials that make terrain readable;
- editor controls for repeated generation.

GeoLandscape combines multiple data sources and converts them into Unreal actors and landscape data.

## Architecture

The plugin is split into two modules:

- `GeoLandscapeRuntime` - generated runtime actors and metadata components.
- `GeoLandscapeEditor` - editor UI, fetchers, builders and generation tools.

Important systems:

- `GeoHeightmapFetcher` - elevation tile/API loading.
- `GeoLandscapeBuilder` - Unreal landscape creation and scaling.
- `GeoUrbanVectorFetcher` - OSM/Overpass loading for roads and buildings.
- `GeoUrban` - city, road and building generation.
- `GeoWaterVectorFetcher` and `GeoWater` - OSM water data and water generation.
- `GeoAutoMaterial` - generated landscape material setup.
- `GeoEnvironment` - sky, light, fog and post process setup.
- `SGeoLandscapeWindow` and `SGeoMapPicker` - editor UI.

## Data Sources

| Source | Used for |
|---|---|
| Nominatim | City search and geocoding |
| OpenStreetMap / Overpass | Roads, buildings, water objects, city data |
| AWS Terrarium | Heightmap tiles |
| Mapbox Terrain-RGB | Heightmap tiles |
| Google Elevation API | Elevation sampling |
| Unreal Water plugin | Lakes and ocean water actors |

## Generation Pipeline

1. User searches a city.
2. Plugin resolves the location and selected boundary.
3. Height data is downloaded and converted into Unreal landscape height values.
4. Landscape is created with correct scale and collision settings.
5. OSM roads/buildings/water are loaded in chunks.
6. Roads are converted into editable spline/mesh actors.
7. Terrain is fitted under roads and urban areas.
8. Buildings are generated from OSM footprints and height tags.
9. Rivers and water areas are processed as riverbeds or Water plugin actors.
10. Landscape material and environment actors are created.

## Roads

Road generation supports:

- OSM `highway` type mapping;
- different widths and profiles;
- spline-based editing;
- generated meshes and optional collision;
- intersections;
- shoulders and curbs;
- optional road markings;
- terrain fitting under road surfaces;
- preservation of selected manual edits after regeneration.

## Buildings

Building generation supports:

- OSM footprints;
- `height` and `building:levels` tags;
- building type metadata;
- editable `GeoBuildingActor` objects;
- batch and sector generation for large city areas;
- preserving deleted or manually changed buildings between regeneration passes.

## Water

Water generation supports:

- OSM `waterway`;
- OSM `natural=water`;
- riverbed cutting in the heightmap;
- lake and ocean generation through the Unreal Water plugin;
- separation between small terrain riverbeds and larger water actors.

## Result

The project demonstrates practical Unreal editor tooling:

- external API integration;
- geodata conversion;
- generated actor workflows;
- terrain and mesh generation;
- editor UI;
- large-scene performance constraints;
- preserving manual edits in generated content.

## Known Limitations

- Public showcase does not include source code because the plugin is private.
- Generated quality depends on OSM data completeness for a selected city.
- Very large areas require careful limits, chunking and batch generation.
- Road intersections and water handling still need polishing in complex edge cases.
