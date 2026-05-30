# Media Guide

This guide is for preparing screenshots and a short demo video for the public GeoLandscape showcase.

## Screenshot Requirements

Use 1920x1080 or higher. Avoid screenshots with:

- API keys;
- local Windows user paths;
- private project names that should not be public;
- crash logs;
- unrelated editor tabs;
- personal accounts or tokens.

## Recommended Screenshots

Current files in `docs/images/`:

1. `01-playable-city-view.png` - playable view with landscape, generated city and character scale.
2. `02-editor-window.png` - `Window -> GeoLandscape` with search/settings visible.
3. `03-generation-map-settings.png` - OSM map, selected area, settings and progress log.
4. `04-landscape-water-overview.png` - generated island/coast terrain and water.
5. `05-city-layout-buildings.png` - generated city layout from OSM building footprints.
6. `06-editable-building-actor.png` - selected generated actor in Unreal Editor.

Recommended additional screenshots:

7. `07-roads-closeup.png` - roads, intersections, curbs and terrain fitting.
8. `08-water-closeup.png` - rivers, lake/ocean and shoreline.

## Demo Video

Recommended length: 30-90 seconds.

Suggested structure:

1. Open `Window -> GeoLandscape`.
2. Search a city.
3. Select the area.
4. Start generation.
5. Show generated landscape.
6. Show close-ups: roads, buildings, water.
7. Show one editable generated actor in the Details panel.

Save as:

```text
docs/video/geolandscape-demo.mp4
```

Then add this line to `README.md`:

```md
[Watch demo video](docs/video/geolandscape-demo.mp4)
```

## Capture Tips

- Use a clean Unreal layout.
- Hide unrelated Content Browser folders if possible.
- Use a neutral level name.
- Prefer a bright daytime scene for readability.
- Take both overview and close-up shots.
- Record a short workflow, not a long unedited session.
