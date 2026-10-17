# GeoLandscape for Unreal Engine 5

[English](README.md) | [Русский](README.ru.md)

![Geirangerfjord generated with GeoLandscape](docs/images/v1/hero_fjord.jpg)

**Turn any place on Earth into a streamed World Partition level.** Pick an area on the map,
press **Create region**, and GeoLandscape builds real 1:1 terrain with OpenStreetMap roads,
a city layout, rivers, PCG forests, grass, a distant view for the game and a 30 km horizon,
all inside the Unreal Editor.

- **Get it:** on Fab (the link appears here when the listing is published).
- **Manual:** [English](docs/Manual.md) · [Русский](docs/Manual.ru.md)
- **Help and bug reports:** [GitHub Issues](https://github.com/evG10/geolandscape-showcase/issues)

This repository holds the documentation, screenshots and support for the plugin. The plugin
source code is not public.

![Realistic or hand-painted ground](docs/images/v1/teaser_ground_styles.gif)

## What it does

- **Terrain** from open elevation data: Copernicus GLO-30 worldwide, USGS 3DEP (US,
  1–10 m), swissALTI3D (Switzerland, 2 m), Terrarium, or your own GeoTIFF. The height range
  is chosen per region, so Everest and the Dead Sea both fit.
- **Roads** from OpenStreetMap, conformed to the terrain with the ground levelled under
  them; bridges and tunnels keep their own height.
- **City layout** from real building footprints and heights: a translucent blockout of the
  city and its roads to model your own buildings on.
- **Rivers and lakes** carved into the terrain, plus optional UE Water oceans, lakes and
  rivers, with white-water cascades on steep falls.
- **PCG forests** in groves and glades with understory, editable vegetation palettes and
  landscape grass. Low-poly trees, shrubs, grass and rocks are included.
- **Ground material** with no painting: grass, soil, forest floor, rock, sand, snow and sea
  floor from slope, height and coastline; realistic or hand-painted, switched in one click.
- **Distant view (HLOD)** for cities, roads, forests and terrain, and a **30 km horizon**
  around the region.
- **Large regions** as 2 × 2 km streamed tiles in three passes. Finished passes are saved,
  so a region can be stopped, resumed and extended later.
- Editor UI in **English and Russian**. Demo levels: Mount Everest, the Matterhorn and
  Zermatt, San Francisco and Geirangerfjord.

## Gallery

| | |
|---|---|
| ![The Matterhorn](docs/images/v1/matterhorn.jpg) | ![Geirangerfjord](docs/images/v1/fjord.jpg) |
| The Matterhorn and Zermatt at 1:1 scale | Geirangerfjord: the sea, rivers and forests |
| ![San Francisco city layout](docs/images/v1/city_layout.jpg) | ![Forest and meadow](docs/images/v1/forest_meadow.jpg) |
| San Francisco: OpenStreetMap roads and a city layout | PCG forests, meadows and grass |

![Realistic and hand-painted ground side by side](docs/images/v1/ground_styles.jpg)

## Quick start

1. Install GeoLandscape from Fab and enable it in **Edit → Plugins**.
2. Press **GeoLandscape** on the main toolbar, or **Ctrl+Alt+G**.

   ![The GeoLandscape toolbar button](docs/images/v1/toolbar.png)

3. Search for a place or pick an example, set the width and length in kilometres.
4. Choose what to create and press **Create region**.

   ![The GeoLandscape panel](docs/images/v1/panel.png)

5. When the tiles are done, press **Distant view** before playing.

   ![A region being generated](docs/images/v1/panel_generating.png)

The [manual](docs/Manual.md) covers every option, the console commands, performance and
troubleshooting.

## Requirements

- Unreal Engine 5.8, Windows 64-bit editor.
- A C++ project, or a Blueprint project with one C++ class added.
- An internet connection while generating; downloaded data is cached for later runs.
  Packaged games contain only the generated actors.
- Recommended: 32 GB of RAM and an SSD for regions larger than about 10 × 10 km.
- Engine plugins PCG, Water, Procedural Mesh Component and GeoReferencing (enabled
  automatically).

## Known issues

- **UE Water near the camera in play.** In Play In Editor and Movie Render Queue, the UE
  Water surface may be missing within about 1 km of the camera; farther away it is drawn,
  and the editor viewport shows it correctly. UE Water is optional and off by default.
- **Trees above the tree line.** Vegetation areas from OpenStreetMap on high mountains can
  place trees on snow.

Both are planned for the next update.

## Support

Open an [issue](https://github.com/evG10/geolandscape-showcase/issues/new/choose) for
questions and bug reports. Please include the engine version, the place and size of the
region, and the Output Log lines that start with `LogGeoLandscape`. Search the existing
issues first: your question may already have an answer.

## Data credits

Generated levels contain data derived from OpenStreetMap (© OpenStreetMap contributors,
ODbL) and from the elevation source you choose, so your project must credit them.
[THIRD_PARTY_NOTICES](docs/THIRD_PARTY_NOTICES.md) lists the credit lines and terms.

## About this repository

GeoLandscape is developed by [evG10](https://github.com/evG10), with Claude, an AI assistant
made by [Anthropic](https://www.anthropic.com), as co-author of the code and documentation.
The plugin source code is private; this repository is its public home for documentation, media and support. The
earlier development write-up is in [docs/CASE_STUDY.md](docs/CASE_STUDY.md).
